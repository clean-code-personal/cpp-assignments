# Encapsulating the Image

Scoping the internals of an Image within the Image itself - so the consumers don't need to worry about its implementation, lifetime, etc. Try `shared_ptr` to manage lifetimes. Use `unique_ptr` only when you are sure of the ownership, like in a "chain of command" pattern.

>Use of `shared_ptr` _doesn't_ guarantee "no" memory leaks! Think of adding it to a collection and leaving it there; or [cyclic references](https://stackoverflow.com/questions/27085782/how-to-break-shared-ptr-cyclic-reference-using-weak-ptr)

## Encapsulation helps in saving resources

[Repo link](https://github.com/clean-code-personal/image-encapsulation-anantharamansekar)
- In case of an invalid image, no need to initialize the `ImageBrightener` (same as the guideline "instantiate as local as possible")
- Can call `use_count` to check the reference count inside a `shared_ptr`

```cpp
	auto image = std::make_shared<Image>(512, 512);
	std::cout << "Brightening a 512 x 512 image\n";
	std::cout << image.use_count() << std::endl;

	if (image->ValidateImage()) {
		ImageBrightener brightener(image);
		std::cout << "After Image brightener instantiation" << image.use_count() << std::endl;
        // ...
    }
```

>Use of `shared_ptr` doesn't guarantee "no" memory leaks! You could add it in a collection and leave it there; or make a [cyclic references](https://stackoverflow.com/questions/27085782/how-to-break-shared-ptr-cyclic-reference-using-weak-ptr)

## Return code from main

[Repo link](https://github.com/clean-code-personal/image-encapsulation-PN-Aditya). Returning an integer from `main()` helps in conveying success/failure to the calling process.
Useful in orchestration frameworks like CICD chains.

```cpp
int main() {
    auto image = std::make_shared<Image>(512, 512);
    if (image->ValidateImage()) {
        // ...
        return 0; // 0 means the work is completed!
    } else {
        std::cout << "Not a valid image... did nothing\n";
        return 1; // non-zero means something went wrong
    }
}
```
