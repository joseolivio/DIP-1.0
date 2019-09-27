# DIP-1.0
Digital image processing 1.0


Some functions implemented:
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





