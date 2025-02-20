import cv2
import os
import numpy as np

# ye image read kareega
img = cv2.imread("doremon.jpg")

# Image Format Check
if img is None:
    print("Error: Image not found!")
    exit()

if len(img.shape) < 3:
    print("Error: Image is not in RGB format!")
    exit()

# Secret Message Input lein
msg = input("Enter secret message: ")
password = input("Enter a passcode: ")

# ASCII Mapping
d = {chr(i): i for i in range(255)}
c = {i: chr(i) for i in range(255)}

# Maximum Message Length Check
max_length = img.shape[0] * img.shape[1] * 3
if len(msg) > max_length:
    print("Error: Message too long for this image!")
    exit()

# Encoding Process
h, w, _ = img.shape
index = 0
for i in range(h):
    for j in range(w):
        if index < len(msg):
            img[i, j, index % 3] = d[msg[index]]
            index += 1
        else:
            break

cv2.imwrite("encryptedImage.jpg", img)
os.system("start encryptedImage.jpg")  # Windows ke liye image open karega

# Decryption Process
message = ""
index = 0

pas = input("Enter passcode for Decryption: ")
if password == pas:
    for i in range(h):
        for j in range(w):
            if index < len(msg):
                message += c[img[i, j, index % 3]]
                index += 1
            else:
                break
    print("Decrypted message:", message)
else:
    print("YOU ARE NOT AUTHORIZED")
