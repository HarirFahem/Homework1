# HSLA Image color space

## **Introduction** 
#### “” **The only way to learn a new programming language is by writing programs in it.**””
The **HSLA** define colors using the Hue-saturation-lightness-alpha model.
**HSLA color** values are an extension of HSL color values with an alpha channel which specifies the opacity of the color.
The **hue** is a degree on the color wheel from 0°to 360°. The value of pixel of an image is represented by the RGB color system (0 is Red, 120 is Green, 240 is blue).
The **saturation** is a percentage value, 0% is a shade of gray, and 100% is the full color, and it can be described as the intensity of a color:
* 100% is pure color, no shades of gray
*	50% is 50% of gray, but the color is still visible 
*	0% is completely gray, the color is no longer visible

Also, **the lightness** is a percentage value, 0% is black and 100% is white. It can be described as how much light you want to give the color:
*	0% is no light(black)
*	50% is 50% light (neither dark for light)
*	100% is full lightness (white)

The **HSL color space** defines colors more naturally: Hue specifies the base color, the other two values then let you specify the saturation of that color and how bright the color should be.
The **full HSL color** space is a three-dimensional space, but it is not a cube.
The **report** sets out to apply different themes (lighten, grayscale, illini and spotlight) to an image by making variations in the lightning, the saturation and the hue.

## **The PNG class**
In the [image_manip.zip](https://github.com/HarirFahem/Homework1/edit/main/README.md) you are given a project which contain a class called PNG that implement basic images manipulation such as Reading Writing and Accessing pixels.
Here is a glance for this class header:




```class PNG{
    PNG();   //default constructor
    PNG(int, int): //constructor with width and height
    ~PNG();         //Destructor
    bool readFromFile(string);  //read from a file
    bool writeToFile(string);  //write content to a file
    HSLAPixel  getPixel(int x, int y); //get content for pixel x, y
};
```



# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/HarirFahem/Homework1/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
