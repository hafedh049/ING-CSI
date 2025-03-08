### **📌 Explication du Code - Transformation en Ondelettes Discrète (DWT)**

Ce script applique la **Décomposition en Ondelettes Discrète (DWT)** sur une image en niveaux de gris à l’aide de la **transformée de Haar** pour extraire ses composantes fréquentielles.

#### **1️⃣ Bibliothèques utilisées**

Avant d’exécuter ce code, assurez-vous d’avoir installé les bibliothèques nécessaires :

```bash
pip install numpy matplotlib pillow pywavelets
```

- **numpy** : Manipulation de tableaux.
- **matplotlib** : Affichage et visualisation des résultats.
- **pywt (PyWavelets)** : Fournit les outils pour effectuer la **Transformation en Ondelettes Discrète (DWT)**.
- **PIL (Pillow)** : Chargement et traitement des images.

---

### **2️⃣ Explication du Code**

```python
import numpy as np
import matplotlib.pyplot as plt
import pywt
from PIL import Image
```

📌 **Importation des bibliothèques nécessaires.**

```python
image_path = "./img.jpg"
img = Image.open(image_path).convert('L')  # Convertir en niveau de gris
img = img.resize((256, 256))  # Redimensionner l'image si nécessaire
```

📌 **Chargement de l’image** et conversion en **niveaux de gris**.  
📌 **Redimensionnement** en 256x256 pour homogénéiser les dimensions (optionnel).

```python
coeffs = pywt.wavedec2(np.array(img), 'haar', level=2)
```

📌 **Application de la Décomposition en Ondelettes Discrète (DWT)** avec **l’ondelette de Haar**.  
Le paramètre `level=2` signifie qu’on effectue **deux niveaux de décomposition**.

---

### **3️⃣ Affichage des Résultats**

On génère une visualisation **élégante et informative** pour mieux comprendre la décomposition.

```python
plt.figure(figsize=(12, 6))
```

📌 Création d’une figure avec une largeur de **12** et une hauteur de **6**.

```python
plt.subplot(3, 3, 1)
plt.imshow(img, cmap='gray')
plt.title("Image originale")
plt.axis('off')
plt.text(0, -10, f'Taille: {img.size}', color='blue', fontsize=8)
```

📌 **Affichage de l’image originale** en niveaux de gris avec ses dimensions.

```python
plt.subplot(3, 3, 2)
plt.imshow(coeffs[0], cmap='gray')
plt.title("Approximation (Niveau 1)")
plt.axis('off')
plt.text(0, -10, f'Taille: {coeffs[0].shape}', color='blue', fontsize=8)
```

📌 **Affichage de l’approximation du premier niveau** :  
Cette partie **contient les basses fréquences**, qui représentent la structure globale de l’image.

---

#### **🔹 Affichage des Détails Ondelettes**

Chaque niveau de décomposition produit **trois sous-bandes** :

- **Détails Horizontaux** (haute fréquence en x)
- **Détails Verticaux** (haute fréquence en y)
- **Détails Diagonaux** (haute fréquence en xy)

```python
for i, detail in enumerate(coeffs[1:]):
    plt.subplot(3, 3, i * 3 + 3)
    plt.imshow(detail[0], cmap='gray')
    plt.title(f"Detail (Niveau {i+1}) Horizontal")
    plt.axis('off')
    plt.text(0, -10, f'Taille: {detail[0].shape}', color='blue', fontsize=8)
    
    plt.subplot(3, 3, i * 3 + 4)
    plt.imshow(detail[1], cmap='gray')
    plt.title(f"Detail (Niveau {i+1}) Vertical")
    plt.axis('off')
    plt.text(0, -10, f'Taille: {detail[1].shape}', color='blue', fontsize=8)
    
    plt.subplot(3, 3, i * 3 + 5)
    plt.imshow(detail[2], cmap='gray')
    plt.title(f"Detail (Niveau {i+1}) Diagonal")
    plt.axis('off')
    plt.text(0, -10, f'Taille: {detail[2].shape}', color='blue', fontsize=8)
```

📌 **Affichage des sous-bandes d’ondelettes** :  
Chaque **niveau de détail** met en évidence les **textures et contours** sous différents axes.

```python
plt.tight_layout()
plt.show()
```

📌 **Ajustement automatique** des sous-graphiques et affichage final.

---

### **🎯 Résultat attendu**

Le script affiche une **matrice d’images** :

1. **L’image originale**.
2. **L’image approximative après DWT (basses fréquences)**.
3. **Les détails horizontaux, verticaux et diagonaux pour chaque niveau de décomposition**.