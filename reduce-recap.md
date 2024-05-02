# Reduce code - recap

## Reducing duplication in test code

[Extract method to create an image](https://github.com/clean-code-personal/reduce-code-HariPhilips/blob/9e4657e2d2bb061976938c10f1453cf765471149/brightening-test/brightening-test.cpp)

## The drive for conventions

...interferes with the function semantics?

```cpp
bool BrightenWholeImage(std::shared_ptr<Image> inputImage, int& attenuatedCount, std::shared_ptr<Image> imageToAdd = nullptr); // imageToAdd must not be passed here, it's present for convention

 bool AddBrighteningImage(std::shared_ptr<Image> inputImage, const std::shared_ptr<Image>& imageToAdd, int& attenuatedCount);
```

## Multiple pixel processors

[Pixel processors with capture groups](https://github.com/clean-code-personal/reduce-code-Sriranganatha1979/blob/479f3fb579d4c262b9f8ad50be70d42a9263585a/brightener.cpp)

Shared pointers with capture group:

```cpp
bool ImageBrightener::AddBrighteningImage(const std::shared_ptr<Image> imageToAdd, int& attenuatedPixelCount) {
    m_inputImage->pixelRunner([&attenuatedPixelCount, imageToAdd](uint8_t pixelGrayscale, int pixelIndex) { 
    ...
```

## Notice the duplication

```cpp
    if (outputGrayscale > (255 - 25)) {
        ++attenuatedPixelCount;
        outputGrayscale = 255;
    } else {
        outputGrayscale += 25;
    }
```

```cpp
    uint8_t pixelsToAdd = imageToAdd->pixels[pixelIndex];
    if (outputGrayscale > (255 - pixelsToAdd)) {
        ++attenuatedPixelCount;
        outputGrayscale = 255;
    } else {
        outputGrayscale += pixelsToAdd;
    }
```

## Shift-left to the compiler using const

```cpp
// the shared_ptr is const, but not its reference
bool ImageBrightener::AddBrighteningImage(const std::shared_ptr<Image> imageToAdd, int& attenuatedCount) {
    // getPixels is a non-const member function.
    // adding to the wrong image by mistake...
    imageToAdd->getPixels()[pixelIndex] += m_inputImage->getPixels()[pixelIndex];
    // no compiler error!
}
```

```cpp
// the referred item is const
bool ImageBrightener::AddBrighteningImage(std::shared_ptr<const Image> imageToAdd, int& attenuatedCount) {
    // adding to the wrong image by mistake...
    imageToAdd->getPixels()[pixelIndex] += m_inputImage->getPixels()[pixelIndex];
    // Yes :) compiler error!
}
```
