# Types for mistake-proofing - recap

Specific types for Raw and Brightened, + [keeping a base type](https://github.com/clean-code-personal/types-for-mistake-proofing-srivathsa-sarvothama/blob/813a73a1c503f05aef3ccf5113eed54936043340/image.h)

## Member functions as const

When a function doesn't alter data-members, make it const

```cpp
bool imageSizeIsValid() const { // add const after the function declaration
    return m_rows <= 1024 && m_columns <= 1024;
}
```

>Careful! Don't add `const` when the function returns non-const reference to data-members

## Reduction of duplication

[Using nested lambdas](https://github.com/clean-code-personal/types-for-mistake-proofing-Sriranganatha1979/blob/a5c90d0b4915b3e1da5c82a6676bee142a0ef291/brightener.cpp)

>Tip: ask ChatGPT with a prompt like "Remove the duplication in this code", followed by th eentire contents of `brightener.cpp`

## Use of types in exceptions

Throwing an "exception type" (instead of a string) helps in catching specific exceptions

```cpp
throw std::invalid_argument("Mismatch between rows and columns.\n");
```

## Encapsulation

`attenuatedPixelCount` is a non-const reference. Maybe an opportunity for improvement.

Perhaps it helps if we move it into a place where it belongs... it's the number of pixels attenuated when brightening the image. What if we keep it together with `BrightenedImage`?
