# Code reduction with lambdas

## Is a class necessary

```cpp
class ImageBrightener {
private:
    std::shared_ptr<Image> m_inputImage;
public:
    ImageBrightener(std::shared_ptr<Image> inputImage);
    int BrightenWholeImage();
};
```
...or...
```cpp
namespace ImageProcessing {
    int BrightenWholeImageWithLambda(std::shared_ptr<Image> image);
}
```

## Pick the odd one out

```cpp
    const uint16_t m_rows;
    const uint16_t m_columns;
    uint8_t* pixels; // max 1k x 1k image
```

