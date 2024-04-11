# Passing an image

Local variables are placed on stack. When large objects (like images) are placed on stack, it can overflow. They can be placed on the heap using allocation operators like `new` or `malloc`.

## Allocating the image on the heap

```cpp
    Image* image = new Image;
    image->rows = 512;
    image->columns = 512;
	std::cout << "Brightening a 512 x 512 image\n";
    ImageBrightener brightener(*image);
```

>Solves the issue. However, consumers need to be aware of the size of the Image object, and manage its lifetime.

## Handling pixels within the image

```cpp
struct Image {
	int rows;
	int columns;
	uint8_t* pixels = new uint8_t[1024 * 1024];
};
```

>We remove restriction on the consumer's stack, but how do we free up the pixels?

## Use unique_ptr to hand-over responsiblity

[Repo link](https://github.com/clean-code-personal/pass-an-image-Elango0811)

```cpp
// accept a unique_ptr
ImageBrightener(std::unique_ptr<Image> inputImage);

// use make_unique instead of new
auto image = std::make_unique<Image>(512, 512);

// pass by "moving" the responsibility of the object
ImageBrightener brightener(std::move(image));
```

## Use unique_ptr only to manage lifetime

[Repo link](https://github.com/clean-code-personal/pass-an-image-devdatt-ka)

```cpp
// accept a unique_ptr reference
ImageBrightener::ImageBrightener(const std::unique_ptr<Image>& inputImage)

// Initialize `unique_ptr` using new
const std::unique_ptr<Image> image(new Image);

// pass the unique_ptr without handing over responsibility
ImageBrightener brightener(image);
```
