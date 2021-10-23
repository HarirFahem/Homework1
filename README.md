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
In the [image_manip.zip](https://github.com/HarirFahem/Homework1/blob/719aeb8bf300e6c1113b32c09e3a74910d8dc155/fahem_harir_image%20(1).zip) we have a project which contain a class called PNG that implement basic images manipulation such as Reading Writing and Accessing pixels.
Here is a glance for this class header:




```javascript
class PNG{
    PNG();   //default constructor
    PNG(int, int): //constructor with width and height
    ~PNG();         //Destructor
    bool readFromFile(string);  //read from a file
    bool writeToFile(string);  //write content to a file
    HSLAPixel  getPixel(int x, int y); //get content for pixel x, y
};
```


## **Inheritance diagram** 
![Diagramme](https://raw.githubusercontent.com/HarirFahem/Homework1/main/diagramme.png)
                  UML class diagram for the additional Images classes.

### Image
We created a class named Image that inherits from the PNG class. 
We added the following methods: 
 *Image (string filename): a special constructor that loads the image from the given filename.
 *lighten (double amount) changes the luminance of each pixel by amount.
Here is the code for the Image class:
```javascript
class Image : public PNG
{
public:
    using PNG :: PNG;
    //method
    Image(string filename);
    void lighten(double amount=0.1);
    virtual void saturate(double amount=0.1);
    virtual void rotateColor(double angle);
};
```
And here is the implementation of these methods :
```javascript
#include "image.h"

Image::Image(string filename) : PNG()
{
    //lecture à partir d'un fichier
 readFromFile(filename);
}
void Image::lighten(double amount)
{
    for(unsigned i=0;i<width();i++)
        for(unsigned j=0;j<height();j++)
        {
            HSLAPixel &P=getPixel(i,j);
            P.l += amount * P.l;
            P.l = (P.l>0)?P.l:0;
            P.l = (P.l<=1)?P.l:1;
        }
}
void Image::saturate(double amount)
{
    for(unsigned i=0;i<width();i++)
        for(unsigned j=0;j<height();j++)
        {
            HSLAPixel &P=getPixel(i,j);
            P.s += amount * P.s ;
            P.s  = (P.s>0)?P.s :0;
            P.s  = (P.s<=1)?P.s :1;
        }
}
void Image::rotateColor(double angle){
    for(unsigned i=0;i<width();i++)
        for(unsigned j=0;j<height();j++)
        {
            HSLAPixel &P=getPixel(i,j);
            P.h +=angle;
            while(P.h>360){
                P.h -= 360;
            }
            while(P.h<0){
                P.h += 360;
            }


        }
}
```
And for showing this result we wrote in the main class the following code:
*  For the lightning:
   Effect of adding 0.4 light on the image:
``` javascript
int main() {
   Image IM("res/euromed_image.png");
    IM.lighten(.4);
    IM.writeToFile("res/image_change.png");

    return 0;
}
```

 ![real image](/euromed_image.png) Effect of adding 0.4 light on the image: ![change image](/image_change.png) 

 * For the saturation:
 ``` javascript
   int main() {
   Image IM("res/euromed_image.png");
    IM.saturate(.9);
    IM.writeToFile("res/image_change.png");
    return 0;
}
```
 ![real image](/euromed_image.png) Effect of adding 0.9 light on the image: ![change image](/image_change 1.png) 

* For the rotateColor:
 ```javascript
    int main() {
   Image IM("res/euromed_image.png");
    IM.rotateColor(99);
    IM.writeToFile("res/image_change.png");
    return 0;
} 
```
  ![real image](/euromed_image.png) Effect of rotating the image by 99 degrees ![change image](/image_change 2.png) 
  
  ### Grayscale
  We wrote a simple class called Grayscale that inherits from Image class. It eliminates all the colors and represents the image using only grayscale level.
Here is the code for the Grayscale class:
```javascript
#include "image.h"

class GrayScale : public Image
{
public:
    using Image::Image;
    GrayScale(string filename);
};
```
And here is the implementation of these methods:
```javascript
#include "grayscale.h"
GrayScale::GrayScale(string filename):Image()
{
    readFromFile(filename);

    Image::saturate(-0.9);
}
```
And in the main class, we wrote those lines to call the methods and showing the result:
```javascript
int main() {
GrayScale G("res/euromed_image.png");
    G.writeToFile("res/image_change.png");
  return 0;
}
```
 ![real image](/grayscale_image.png) Here , we are showing the effect of reducing the saturation of each pixel : ![change image](/image_change 3.png) 
 
 ### Illini
 We created a class named Illini that inherits from the Image class. An illini image has only two colors that are defined as attributes (color1 and color2), the constructor accepts these two colors.   It replaced he hue of every pixel is set to the hue value of either orange or blue, based on if the pixel's hue value is closer to orange than blue.
Here is the code for the Illini class:
```javascript
#include <image.h>

class Illini: public Image
{
public:
    int color1;
    int color2;
    Illini(string filename);
    using Image::Image;
    Illini(string filename, int color1, int color2);
    void blueorange();
};
```
And here is the implementation of these methods:
```javascript
#include "illini.h"
Illini::Illini(string filename):Image()
{
    readFromFile(filename);
}
Illini::Illini(string filename, int color1, int color2):Image(filename)
{
    this->color1=color1;
    this->color2=color2;
}
void Illini::blueorange(){
    for (unsigned x = 0; x < width(); x++) {
        for (unsigned y = 0; y < height(); y++) {
          HSLAPixel & P =getPixel(x, y);
          color1=11;
          color2=216;
          if (abs(P.h - color1) < abs(P.h - color2)) {
              P.h = color1;
          }
          else {
              P.h = color2;
          }
        }
      }
}
```
And here is the part of the main class:
```javascript
int main() {
Illini I("res/illini_image.png");
    I.blueorange();
    I.writeToFile("res/image_change.png");
return 0;
}
```
![real image](/illini_image.png) Here is the illini image which only has two colors : the blue and the orange ![change image](/image_change 4.png) 

### Spotlight
We created a class called spotlight and returns an image with a spotlight centred at (centerX and centerY).
A Spotlight image create a spotlight centered at a given point centerX, centerY defined as attributes.
A spotlight adjusts the luminance of a pixel based on the Euclidean distance the pixel is away from the center by decreasing the luminance by 0.5% per 1 pixel, up to 80% decrease of luminance.
Here is the code for the Spotlight class:
```javascript
#include <image.h>
class Spotlight:public Image
{
public:
    Spotlight(string filename,int centerX, int centerY);
    using Image::Image;
    void changeSpotPoint(int centerX, int centerY);
private:
    int xCenter;
    int yCenter;
};
```
And here is the implementation of these methods:
```javascript
#include "spotlight.h"
#include <math.h>

Spotlight::Spotlight(string filename, int centerX, int centerY):Image()
{
int x=0,y=0;
    readFromFile(filename);
    this->xCenter = centerX;
    this->yCenter = centerY;
    for (unsigned i = 0; i < width(); i++) {
            for (unsigned j= 0; j <height(); j++) {
            HSLAPixel & P=getPixel(i, j);
            x=i-centerX;
            y=j-centerY;
            int d=sqrt((x*x)+(y*y));

           if(d<160)
                 P.l=P.l-(0.5*d/100)*P.l;
           else
                 P.l = P.l - 0.8*P.l;
      }
    }
}

void Spotlight::changeSpotPoint(int centerX, int centerY){
    for (int x = 0; x < width(); ++x){
        for (int y = 0; y < height(); ++y){
            HSLAPixel & P = getPixel(x, y);
            auto oldd = sqrt(pow(x-this->xCenter,2) + pow(y-this->yCenter,2));
            auto newd = sqrt(pow(x-centerX,2) + pow(y-centerY,2));
            if (oldd < 160 || newd < 160){
                if(newd > 160)
                  P.l *=.2;
                else
                  P.l *= 1-.005*newd;
                if(oldd > 160)
                  P.l /=.2;
                else
                  P.l /= 1-.005*oldd;
            }
        }
    }
    this->xCenter = centerX;
    this->yCenter = centerY;
}
```
And for the mainly part:
```javascript
int main() {
Spotlight S("res/euromed_image.png",300,500);
    S.changeSpotPoint(600,600);
    S.writeToFile("res/image_change.png");return 0;
}
```
![real image](/euromed_image.png) Here is the illustration of spotlight effect: ![change image](/image_change 5.png) 


















