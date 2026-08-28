# Sunglasses Overlay Using OpenCV

## Name: Chintala Aman Monty
## Reg No: 212224040054

## Aim

To overlay a transparent PNG image of sunglasses onto a face image using OpenCV by utilizing the alpha channel for seamless image blending.

---

# Software Used

- Python 3.x
- OpenCV (cv2)
- NumPy
- Matplotlib
- Jupyter Notebook

---

# Algorithm

1. Import the required libraries.
2. Load the face image.
3. Load the sunglasses PNG image with its alpha (transparency) channel.
4. Resize the sunglasses image to fit the face.
5. Separate the color channels and alpha channel of the sunglasses image.
6. Display the color image and alpha mask.
7. Create a 3-channel mask from the alpha channel.
8. Extract the Region of Interest (ROI) corresponding to the eye region.
9. Blend the sunglasses with the face image using the alpha mask.
10. Replace the eye region with the blended output.
11. Display the original and final images.

---

# Program

## Step 1: Import Required Libraries

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

---

## Step 2: Load the Face Image

```python
faceImage = cv2.imread('monty.jpeg')
plt.imshow(faceImage[:,:,::-1])
plt.title("Face")
```

### Output

<img width="475" height="532" alt="image" src="https://github.com/user-attachments/assets/0ea181ab-70d3-4f10-a110-e993fef59fff" />

---


## Step 3: Load the Sunglasses PNG

```python
glassPNG = cv2.imread('sunglass.png', -1)
plt.imshow(glassPNG[:,:,::-1])
plt.title("Sunglasses")
```

### Output

<img width="810" height="380" alt="image" src="https://github.com/user-attachments/assets/6d754e45-af45-41b1-90ef-fa26eb10f604" />


---
## Step 4: Resize the image

```python
faceImage = cv2.resize(faceImage, (1600, 1900))
plt.imshow(faceImage[..., ::-1])
plt.axis('off')
plt.show()
```

## Step 5: Resize the Sunglasses

```python
glassPNG = cv2.resize(glassPNG, (820,300))
print(glassPNG.shape)
```

## Step 6: Separate the Color and Alpha Channels

```python
glassBGR = glassPNG[:,:,0:3]
glassMask1 = glassPNG[:,:,3]
```

Display both channels:

```python
plt.figure(figsize=(8, 10))
plt.imshow(faceWithGlassesNaive[..., ::-1])
plt.axis('off')
plt.show()# Display the images for clarity
plt.figure(figsize=[15,15])
plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels');
plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel');
```

### Output

<img width="1420" height="287" alt="image" src="https://github.com/user-attachments/assets/e40f78f8-726b-4802-b5d4-c36754ab2bec" />

---

## Step 7: Naive Overlay (Without Transparency)

```python
faceWithGlassesNaive = faceImage.copy()
faceWithGlassesNaive[400:700,410:1230] = glassBGR
plt.imshow(faceWithGlassesNaive[...,::-1])
plt.axis('off')
plt.show()
```

### Output

<img width="462" height="475" alt="image" src="https://github.com/user-attachments/assets/0063e5dc-e084-4400-bd05-ca72ec3b3d2b" />


---

## Step 8: Alpha Blending

```python
glassMask = cv2.merge((glassMask1,glassMask1,glassMask1))
glassMask = np.uint8(glassMask/255)
faceWithGlassesArithmetic = faceImage.copy()
eyeROI= faceWithGlassesArithmetic[400:700,410:1230]
maskedEye = cv2.multiply(eyeROI,(1-  glassMask ))
maskedGlass = cv2.multiply(glassBGR,glassMask)
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)
plt.figure(figsize=[20,20])
plt.subplot(131);plt.imshow(maskedEye[...,::-1]);plt.title("Masked Eye Region")
plt.subplot(132);plt.imshow(maskedGlass[...,::-1]);plt.title("Masked Sunglass Region")
plt.subplot(133);plt.imshow(eyeRoiFinal[...,::-1]);plt.title("Augmented Eye and Sunglass")
```

### Output

<img width="1417" height="242" alt="image" src="https://github.com/user-attachments/assets/a2f905f9-fe2f-4a51-bdfc-673b7545eef4" />

---

## Step 9: Display the Final Result

```python
faceWithGlassesArithmetic[400:700,410:1230] = eyeRoiFinal
plt.figure(figsize=(20,20))
plt.subplot(121)
plt.imshow(faceImage[...,::-1])
plt.title("Original Image")
plt.subplot(122)
plt.imshow(faceWithGlassesArithmetic[...,::-1])
plt.title("With Sunglasses")
plt.show()
```

### Output
<img width="1393" height="755" alt="image" src="https://github.com/user-attachments/assets/a66bc084-911d-4453-b209-ed18aaedbecf" />

---

# Result

The transparent PNG sunglasses image was successfully overlaid onto the face image using OpenCV's alpha blending technique. The alpha channel preserved the transparency of the sunglasses, resulting in a realistic overlay where the frames, lenses, and side arms blend naturally with the face.
