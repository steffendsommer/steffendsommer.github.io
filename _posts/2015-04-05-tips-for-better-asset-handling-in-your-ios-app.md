---

layout: post
title:  "Tips for better asset handling in your iOS app"
date:   2015-04-05 16:43:03
group: blog
summary: Even though I like the idea of not having to handle image assets, it's often not a viable solution. Asset handling can easy be overlooked in a app that grows in complexity. Here are some of my suggestions for handling your assets better.

---

# 1. Let Xcode generate 1x, 2x and 3x PNG's
With the launch of Xcode 6, Apple provided a way for us to store assets as PDF files. Xcode will then handle the creation of 1x, 2x and 3x PNG files at build time. Convenient, but may not always be the best solution depending on your assets (as [highlighted here](http://bjango.com/articles/idontusepdfs/){:target="_blank"}). The recent apps I've worked on had really simple assets without any shadows or gradients, and PDF files worked as a charm. Martin Craft has a [fine description](http://martiancraft.com/blog/2014/09/vector-images-xcode6/){:target="_blank"} on how to get your started with PDF files in Xcode.

# 2. Color your own assets
Sometimes as a developer, you're given the entire design for the app you're making. This is often not the case. Things change, such as design, placement of elements, colours and icons. That's why I started experimenting with colouring all of my assets in code. This means time saved on the UX/graphics guy and an increase of flexibility in your app.

Of course, all assets are not equally easy to color and some may not be appropriate for programatically being coloured at all. As mentioned, the last apps I've been working on had really simple "flat-ish" design, with no gradients or shadows. In this case, this approach worked really good.

For colouring assets, you might want to bring in an external library such as UIImage+Additions, if you're going to need all of that functionality. You could also just make your own small colouring method. Here is something to get you started:

``` objc
- (UIImage *)colorImage:(UIImage *)image color:(UIColor *)color
{
    UIGraphicsBeginImageContextWithOptions(image.size, NO, [UIScreen mainScreen].scale);
    CGContextRef context = UIGraphicsGetCurrentContext();

    CGContextTranslateCTM(context, 0, image.size.height);
    CGContextScaleCTM(context, 1.0, -1.0);
    CGRect rect = CGRectMake(0, 0, image.size.width, image.size.height);

    CGContextSetBlendMode(context, kCGBlendModeNormal);
    CGContextDrawImage(context, rect, image.CGImage);
    CGContextSetBlendMode(context, kCGBlendModeSourceIn);
    [color setFill];
    CGContextFillRect(context, rect);


    UIImage *coloredImage = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();

    return coloredImage;
}
```

This code will make a simple image asset go from:

![Alt text](/assets/posts/1428243313539.png){:height="50px" width="50px"}

to:

<img src="/assets/posts/1428243326767.png" width="50" height="50">

Pretty simple, right? This will also make your app ready for skinning if that is what you want. If you have assets that contains multiple colours or gradients, you might want to look into tinting properties. Have a look at this SO answer for an example of tinting with UIImageView.

# 3. Make a facade for your assets
I like my code to be strict, in the sense that I want to guide any consumer of my code in the best way I can. One way of making my assets more strict is to make a facade for them. One way to do this, is to make a category on UIImage. Your category can then expose any assets that are allowed to be used throughout your code base. An example could be:

``` objc
+ (instancetype)img_successIcon {
    return [self colorImage:[UIImage imageNamed:@"thumbsup"] color:[UIColor greenColor]];
}
```

Having the facade also centralises the list of assets for your app. This means that if your code uses this category, you only have to update paths to assets in one place. Naming your assets in general terms that makes sense for your app, also contributes to having a more flexible code base. Lastly, the facade can hide away any colouring of the assets.

 

Feel free to get in touch if you have any questions or feedback. As always, I'm interested to improve the way I work.