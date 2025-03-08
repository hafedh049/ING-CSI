Your code applies the Discrete Cosine Transform (DCT) to an image, extracts the most significant coefficients, reconstructs the image using those coefficients, and visualizes the results. Below is the same approach, maintaining the same layout and logic but rewritten with clarity and structure.

### **📌 Explication du Code - Transformation en Cosinus Discrète (DCT)**

Ce script applique la **Transformation en Cosinus Discrète (DCT)** à une image en niveaux de gris, extrait les **coefficients les plus significatifs**, et reconstruit l’image avec ces coefficients.

#### **1️⃣ Bibliothèques utilisées**

Avant d’exécuter ce code, assurez-vous d’avoir installé les bibliothèques nécessaires :

```bash
pip install numpy matplotlib pillow scipy
```

- **numpy** : Manipulation des matrices.
- **matplotlib** : Affichage et visualisation des résultats.
- **PIL (Pillow)** : Chargement et prétraitement des images.
- **scipy.fftpack (DCT & IDCT)** : Implémentation de la **DCT et IDCT** pour la compression et reconstruction d’images.

---

### **2️⃣ Explication du Code**

```python
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
from scipy.fftpack import dct, idct
```

📌 **Importation des bibliothèques nécessaires.**

```python
image_path = "img.jpg"
img = Image.open(image_path).convert('L')  # Convertir en niveau de gris
img = img.resize((256, 256))  # Redimensionner l'image pour homogénéiser les dimensions
```

📌 **Chargement de l’image**, conversion en **niveaux de gris** et redimensionnement en **256x256** (optionnel).

```python
num_coeffs = 200  # Nombre de coefficients DCT les plus significatifs à conserver
img_array = np.array(img)  # Conversion en tableau numpy
```

📌 **Paramètre clé** : Nous gardons uniquement **200 coefficients** parmi les plus importants pour reconstruire l’image.

```python
dct_img = dct(dct(img_array, axis=0, norm='ortho'), axis=1, norm='ortho')
```

📌 **Application de la DCT** en 2D :

- D’abord sur les **lignes** (`axis=0`),
- Ensuite sur les **colonnes** (`axis=1`),
- `norm='ortho'` assure une transformation **orthonormée** pour éviter l’amplification des valeurs.

```python
dct_coeffs = np.zeros_like(dct_img)
idx = np.argsort(np.abs(dct_img.flatten()))[::-1][:num_coeffs]
dct_coeffs.flat[idx] = dct_img.flat[idx]
```

📌 **Sélection des coefficients DCT les plus significatifs** :

- `np.argsort(np.abs(dct_img.flatten()))[::-1]` trie les valeurs en **ordre décroissant**.
- On sélectionne les **200 plus grandes valeurs** et met **tous les autres coefficients à zéro**.

```python
reconstructed_img = idct(idct(dct_coeffs, axis=0, norm='ortho'), axis=1, norm='ortho')
```

📌 **Reconstruction de l’image** via l’**IDCT** en appliquant l’inverse de la transformation sur les coefficients sélectionnés.

---

### **3️⃣ Visualisation des Résultats**

On génère une **visualisation claire** pour comprendre l’effet de la compression.

```python
plt.figure(figsize=(12, 8))
```

📌 **Création d’une figure de taille 12x8 pour afficher les résultats.**

```python
plt.subplot(2, 3, 1)
plt.imshow(img, cmap='gray')
plt.title("Image originale")
plt.axis('off')
plt.text(0, -10, f'Taille: {img.size}', color='blue', fontsize=8)
```

📌 **Affichage de l’image originale.**

```python
plt.subplot(2, 3, 2)
plt.imshow(np.log(np.abs(dct_img) + 1), cmap='gray')
plt.title("Spectre de la DCT")
plt.axis('off')
plt.text(0, -10, f'Taille: {dct_img.shape}', color='blue', fontsize=8)
```

📌 **Affichage du spectre de la DCT (échelle logarithmique)** pour voir la répartition des fréquences.

```python
plt.subplot(2, 3, 4)
plt.imshow(reconstructed_img, cmap='gray')
plt.title(f"Image reconstruite ({num_coeffs} coeffs)")
plt.axis('off')
plt.text(0, -10, f'Taille: {reconstructed_img.shape}', color='blue', fontsize=8)
```

📌 **Affichage de l’image reconstruite** avec les **200 coefficients retenus**.

```python
plt.subplot(2, 3, 5)
plt.imshow(img_array - reconstructed_img, cmap='gray')
plt.title("Erreur de reconstruction")
plt.axis('off')
plt.text(0, -10, f'Taille: {img_array.shape}', color='blue', fontsize=8)
```

📌 **Affichage de l’erreur de reconstruction** (différence entre l’image originale et reconstruite).

```python
plt.tight_layout()
plt.show()
```

📌 **Ajustement automatique** des sous-graphiques et affichage.

---

### **4️⃣ Analyse des Coefficients Sélectionnés**

Le script affiche aussi le **vecteur des coefficients retenus**.

```python
chosen_coeffs = dct_img.flat[idx]
print("Vecteur des coefficients caractéristiques choisis:", chosen_coeffs)
```

📌 **Affichage des 200 coefficients retenus**.

```python
plt.figure(figsize=(10, 4))
plt.stem(chosen_coeffs, use_line_collection=True)
plt.title(f"Vecteur des {num_coeffs} coefficients caractéristiques")
plt.xlabel("Index des coefficients")
plt.ylabel("Valeur")
plt.grid(True)
plt.show()
```

📌 **Affichage graphique** des coefficients sélectionnés sous forme de **diagramme de tiges**.

---

### **🎯 Résultat Attendu**

Le script produit les **5 visualisations suivantes** :

1. **L’image originale**.
2. **Le spectre de la DCT**.
3. **L’image reconstruite en basse fréquence (BF)** (200 coefficients).
4. **L’erreur de reconstruction**.
5. **Les coefficients sélectionnés sous forme de diagramme de tiges**.

---

### **📌 Applications Pratiques**

✅ **Compression d’images (JPEG)** :

- En réduisant le nombre de coefficients, on réduit l’espace mémoire requis.
- Plus on garde de coefficients, plus l’image reconstruite est fidèle.

✅ **Traitement des images & vision par ordinateur** :

- Permet d’extraire des caractéristiques importantes.
- Réduction du bruit et amélioration du contraste.

✅ **Reconnaissance faciale & classification** :

- La DCT extrait les **fréquences dominantes** d’une image, facilitant la détection des motifs.

---

### **📢 Conclusion**

🚀 **Ce code démontre comment appliquer la DCT pour la compression et reconstruction d’images.**  
✨ **Il est particulièrement utile en vision par ordinateur et traitement du signal.**