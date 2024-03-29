# DIP-1.0
Digital image processing 1.0


Some functions implemented, using Python 3...
# Display individual RGB channels
Function to display each pixel value for each component Red, Green, Blue.
```
def display_rgb(photo, rgb):
  display_r = np.zeros(photo.shape)
  display_g = np.zeros(photo.shape)
  display_b = np.zeros(photo.shape)
  display_r = photo[:,:,0]
  display_g = photo[:,:,1]
  display_b = photo[:,:,2]
  if rgb == 'r':
    plt.imshow(display_r, 'Reds')

  elif rgb == 'g':
    plt.imshow(display_g, 'Greens')

  elif rgb == 'b':
    plt.imshow(display_b, 'Blues')

  else:
    plt.figure(figsize=(20,10))

    plt.subplot(1, 3, 1)
    plt.imshow(display_r, 'Reds')

    
    plt.subplot(1, 3, 2)
    plt.imshow(display_g, 'Greens')


    plt.subplot(1, 3, 3)
    plt.imshow(display_b, 'Blues')
``` 

![1](https://user-images.githubusercontent.com/31492509/65737121-dba06300-e0b3-11e9-8f5b-5813f7f51199.png)

# Convert from RGB TO YIQ
```
def from_rgb_to_yiq(photo):
  shape = photo.shape
  yiq = np.zeros(photo.shape)
  for i in range(0,shape[0]):
    for j in range(0,shape[1]):
      yiq[i,j,0] = (0.299*photo[i,j,0] + 0.587*photo[i,j,1] + 0.114*photo[i,j,2]) #Y
      yiq[i,j,1] = (0.596*photo[i,j,0] - 0.274*photo[i,j,1] - 0.322*photo[i,j,2]) #I
      yiq[i,j,2] = (0.211*photo[i,j,0] - 0.523*photo[i,j,1] + 0.312*photo[i,j,2]) #Q
      
  return yiq
  ```
  
  ![2](https://user-images.githubusercontent.com/31492509/65737125-de02bd00-e0b3-11e9-87dc-8363fa4b4260.png)
  
 # Convert from YIQ to RGB
 ```
 def from_yiq_to_rgb(photo):
    shape = photo.shape
    rgb = np.zeros(photo.shape, dtype = int)
    for i in range(0,shape[0]): 
        for j in range(0,shape[1]):
            rgb[i,j,0] = (1.000*photo[i,j,0] + 0.956*photo[i,j,1] + 0.621*photo[i,j,2])
            rgb[i,j,1] = (1.000*photo[i,j,0] - 0.272*photo[i,j,1] - 0.647*photo[i,j,2])
            rgb[i,j,2] = (1.000*photo[i,j,0] - 1.106*photo[i,j,1] + 1.703*photo[i,j,2])
            for c in range(0,3):
                if(rgb[i,j,c]<0):
                    rgb[i,j,c]=0
                if(rgb[i,j,c]>255):
                    rgb[i,j,c]=255
  
    return rgb
```

![3](https://user-images.githubusercontent.com/31492509/65737127-dfcc8080-e0b3-11e9-8523-5803580f03a6.png)

# Negative of the image
Also negative of one of the RGB channels.
```
def negative(photo):
    shape = photo.shape
    negative = np.zeros(photo.shape, dtype = int)
    for i in range(0,shape[0]): 
        for j in range(0,shape[1]):
            negative[i,j,0] = 255-photo[i,j,0]
            negative[i,j,1] = 255-photo[i,j,1]
            negative[i,j,2] = 255-photo[i,j,2]
  
    return negative

def negative_ch(photo,channel):
    negative_ops = {'nr': 0, 'ng':1, 'nb':2}
    shape = photo.shape
    negative = np.zeros(photo.shape, dtype = int)
    for i in range(0,shape[0]): 
        for j in range(0,shape[1]):
            negative[i,j,0] = photo[i,j,0]
            negative[i,j,1] = photo[i,j,1]
            negative[i,j,2] = photo[i,j,2]
            negative[i,j,negative_ops[channel]] = 255-photo[i,j,negative_ops[channel]]
            
    return negative
```
![4](https://user-images.githubusercontent.com/31492509/65737131-e22eda80-e0b3-11e9-947b-9da71656ba62.png)

# Brightness control
This is done by multiplying a constant for each RGB value, you can't let the values go out of the 0 - 255 range, also you can do the brightness for each RGB channel.
```
def brightness_level(photo, brightness = 1):
  shape = photo.shape
  shinebright = np.zeros(photo.shape, dtype = int)
  for i in range(0,shape[0]): 
    for j in range(0,shape[1]):
      shinebright[i,j,0] = min(255, brightness * photo[i,j,0])
      shinebright[i,j,1] = min(255, brightness * photo[i,j,1])
      shinebright[i,j,2] = min(255, brightness * photo[i,j,2])
      
  return shinebright
  
  def brightness_level_ch(photo, brightness = 1, channel = 0):
  shape = photo.shape
  shinebright = np.zeros(photo.shape, dtype = int)
  for i in range(0,shape[0]): 
    for j in range(0,shape[1]):
      shinebright[i,j,channel] = min(255, brightness * photo[i,j,0])
      shinebright[i,j,1] = photo[i,j,1]
      shinebright[i,j,2] = photo[i,j,2]
      
  return shinebright
  ```
  ![5](https://user-images.githubusercontent.com/31492509/65737132-e2c77100-e0b3-11e9-9f38-8eed7db00bed.png)
  
  # Convolution operation
  ```
  def convolution(photo,m,n,mask):
    mask = rebater(mask)
    shape = photo.shape
    #shape = [6,6]
    conv = np.zeros([shape[0]-(m-1),shape[1]-(n-1),3], dtype = int)
    for i in range(0,shape[0]-(m-1)): 
        for j in range(0,shape[1]-(n-1)):
            for i_m in range(0,m):
                for j_n in range(0,n):
                    conv[i,j,0]+=mask[i_m,j_n]*photo[i+i_m,j+j_n,0]
                    conv[i,j,1]+=mask[i_m,j_n]*photo[i+i_m,j+j_n,1]
                    conv[i,j,2]+=mask[i_m,j_n]*photo[i+i_m,j+j_n,2]
        for c in range(0,3):
            if(conv[i,j,c]<0):
                conv[i,j,c]=0
            if(conv[i,j,c]>255):
                conv[i,j,c]=255
    return conv

``` 
  ![6](https://user-images.githubusercontent.com/31492509/65737130-e22eda80-e0b3-11e9-8665-0c134abb4476.png)


