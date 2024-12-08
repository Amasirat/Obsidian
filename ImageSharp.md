
It is a powerful image processing library in C#.

You can save, load and resize existing images

```C#
using (Image image = Image.Load("image_directory"))
{
    image.Mutate(x => x.Resize(image.Width / 2, image.Height / 2)); 

	image.Save("new_image_directory");
}
```

Formats are detected automatically and once it is loaded, it will become a **format-independant** series of pixels.

A generic image class also exists, Image\<TPixel\>, where TPixel can be one of [these pixel types](https://docs.sixlabors.com/api/ImageSharp/SixLabors.ImageSharp.PixelFormats.html#structs)

Rgba32 is a common one which is a struct containing its RGBA values.

This is how to access each pixel. You should use Span\<T> to store a row of pixels.

```C#
using SixLabors.ImageSharp;

// ...
using Image<Rgba32> image = Image.Load<Rgba32>("my_file.png");
image.ProcessPixelRows(accessor =>
{
    // Color is pixel-agnostic, but it's implicitly convertible to the Rgba32 pixel type
    Rgba32 transparent = Color.Transparent;

    for (int y = 0; y < accessor.Height; y++)
    {
        Span<Rgba32> pixelRow = accessor.GetRowSpan(y);

        // pixelRow.Length has the same value as accessor.Width,
        // but using pixelRow.Length allows the JIT to optimize away bounds checks:
        for (int x = 0; x < pixelRow.Length; x++)
        {
            // Get a reference to the pixel at position x
            ref Rgba32 pixel = ref pixelRow[x];
            if (pixel.A == 0)
            {
                // Overwrite the pixel referenced by 'ref Rgba32 pixel':
                pixel = transparent;
            }
        }
    }
});
```

Span however has some limitations:

* It can only live on stack memory
* It can not be the instance field of any type that lives on the heap
* It can not be a generic type argument.


It's important to know that most of the type, ImageSharp's pixel buffers are **discontiguous memory**.


