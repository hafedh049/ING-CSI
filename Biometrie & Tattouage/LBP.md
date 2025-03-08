### **📌 Explication du Code - Extraction des Caractéristiques LBP**

Ce script réalise l'extraction des caractéristiques **Local Binary Patterns (LBP)**, une technique couramment utilisée pour l'analyse de texture et la reconnaissance d'images.

#### **1️⃣ Bibliothèques utilisées**

Pour exécuter ce code, vous devez installer les bibliothèques suivantes (si elles ne sont pas encore installées) :

```bash
pip install numpy matplotlib scikit-image pillow
```

- **numpy** : Manipulation de matrices et tableaux numériques.
- **matplotlib** : Affichage des images et visualisation des résultats.
- **skimage.feature** : Contient la fonction `local_binary_pattern` pour extraire les LBP.
- **skimage.color** : Conversion d'image en niveaux de gris (`rgb2gray`).
- **PIL (Pillow)** : Chargement et traitement des images.

---

### **2️⃣ Explication du Code**

```python
import numpy as np
import matplotlib.pyplot as plt
from skimage.feature import local_binary_pattern
from skimage.color import rgb2gray
from PIL import Image
```

📌 **Importation des bibliothèques nécessaires.**

```python
image_path = r"C:\Users\Hafedh GUENICHI\Desktop\Semester II\Biometrie & Tattouage\II\Méthodes d extractions des Cractéristiques Faciale\Méthodes d extractions des Cractéristiques Faciale\img.jpg"
img = Image.open(image_path)
img_gray = rgb2gray(np.array(img))  # Convertir en niveaux de gris
```

📌 **Chargement de l’image** et conversion en **niveaux de gris** (car LBP fonctionne sur des images en noir et blanc).

```python
radius = 3
n_points = 8 * radius
```

📌 **Définition des paramètres LBP** :

- `radius = 3` → Rayon du voisinage autour de chaque pixel.
- `n_points = 8 * radius` → Nombre de points échantillonnés autour du pixel central.

```python
lbp = local_binary_pattern(img_gray, n_points, radius, method='uniform')
```

📌 **Calcul des LBP** à l’aide de `local_binary_pattern()`.  
Le mode **"uniform"** signifie que seuls les motifs de transition uniforme sont conservés, ce qui réduit la complexité des calculs.

---

### **3️⃣ Affichage des Résultats**

Le script génère une **visualisation élégante** pour mieux interpréter les résultats.

```python
plt.figure(figsize=(12, 6))
```

📌 Création d’une figure avec une largeur de **12** et une hauteur de **6**.

```python
plt.suptitle("Extraction des Caractéristiques LBP pour la Classification d'Images", fontsize=16, fontweight='bold')
plt.figtext(0.5, 0.93, "Ce travail pratique illustre l'utilisation du descripteur LBP\n"
                         "pour extraire les caractéristiques d'une image et effectuer\n"
                         "la classification d'images. Les LBP permettent de capturer\n"
                         "les textures locales dans une image, fournissant ainsi une\n"
                         "représentation robuste pour la reconnaissance d'objets et\n"
                         "la détection de motifs dans des scènes visuelles.", ha='center', va='top', fontsize=10)
```

📌 **Ajout d’un titre principal et d’une description explicative**.

```python
plt.subplot(1, 2, 1)
plt.imshow(img, cmap='gray')
plt.title("Image originale")
plt.axis('off')
```

📌 Affichage de **l’image originale** en niveaux de gris.

```python
plt.subplot(1, 2, 2)
plt.imshow(lbp, cmap='gray')
plt.title("Caractéristiques LBP")
plt.axis('off')
```

📌 Affichage de **l’image transformée** en carte LBP.

```python
plt.tight_layout(rect=[0, 0.03, 1, 0.92])
plt.show()
```

📌 **Ajustement automatique** des sous-graphiques et affichage final.

---

### **🎯 Résultat attendu**

Le script affiche **deux images côte à côte** :

1. **L’image originale** en niveaux de gris.
2. **L’image après application du LBP**, mettant en évidence les textures sous forme de motifs binaires.

✨ **Grâce à cette approche, nous obtenons une représentation robuste des textures locales, utilisée pour la classification d’images, la reconnaissance faciale et bien plus encore.**