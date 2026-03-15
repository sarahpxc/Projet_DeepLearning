# Projet_DeepLearning

# Présentation du projet

L’objectif de ce projet est de déterminer si un drone peut atterrir en sécurité sur une zone d’atterrissage.

Pour cela, nous utilisons un modèle de vision par ordinateur basé sur l’apprentissage profond afin de détecter la présence d’un landing pad dans l’image.

Une fois la zone détectée, un algorithme de décision détermine si l’atterrissage est SAFE ou NOT SAFE en fonction de deux critères :

la confiance de détection du modèle

la taille relative du landing pad dans l’image

Si le landing pad est détecté avec une grrande confiance et qu’il est suffisamment grand dans l’image, l’atterrissage est considéré comme SAFE.

# Dataset

Un dataset d’images contenant des landing pads a été créé et annoté.

Chaque image est associée à un fichier d’annotation contenant la boîte englobante (bounding box) du landing pad.

Le dataset est divisé en trois parties :

train : utilisé pour entraîner le modèle

valid : utilisé pour valider les performances pendant l’entraînement

test : utilisé pour tester le modèle final

Structure du dataset :

dataset/

    train/ 
      images/
      labels/

    valid/  
      images/
      labels/

    test/  
      images/
      labels/

# Modèle utilisé

Le modèle utilisé est YOLOv8.

Afin d’améliorer l’apprentissage avec un dataset limité, nous utilisons le transfer learning à partir d’un modèle pré-entraîné :

yolov8n.pt

# Entraînement du modèle

L’entraînement est réalisé avec la bibliothèque Ultralytics YOLO.

Après l’entraînement, le meilleur modèle est sauvegardé dans :

runs/detect/train/weights/best.pt

# Évaluation du modèle

Les performances du modèle sont évaluées sur le dataset de validation à l’aide des métriques suivantes :

Precision

Recall

mAP (mean Average Precision)

# Algorithme de décision d’atterrissage

Nous utilisons une règle de décision basée sur deux seuils :

τ_conf : seuil minimum de confiance

τ_area : taille minimale du landing pad dans l’image

La taille relative est calculée par :

area_ratio = aire_bounding_box / aire_image

La décision est :

SAFE si :

confidence ≥ τ_conf
ET
area_ratio ≥ τ_area

Sinon :

NOT SAFE

# Détection en temps réel

Le modèle peut également être utilisé avec une webcam pour simuler une caméra embarquée sur un drone.

Le système réalise alors les étapes suivantes :

capture d’une image depuis la webcam

détection du landing pad

calcul de la confiance et de la taille relative

affichage de la décision SAFE / NOT SAFE

L’interface affiche :

la bounding box détectée

la confiance du modèle

la décision d’atterrissage

# Reproduction du projet 

# Installer les dépendances

pip install ultralytics opencv-python pillow pyyaml

# Vérifier l’organisation des fichiers

Le projet doit contenir au minimum les éléments suivants :

projet/
│
├── train/
│   ├── images/
│   └── labels/
│
├── valid/
│   ├── images/
│   └── labels/
│
├── test/
│   ├── images/
│   └── labels/
│
├── data.yaml
├── safe_drone_landing_vscode.ipynb
└── README.md

Le fichier data.yaml doit pointer vers les dossiers du dataset.

Exemple :

train: train/images
val: valid/images
test: test/images

nc: 1
names: ['landing_pad']

# Ouvrir le notebook

Ouvrir le fichier :

safe_drone_landing_vscode.ipynb

# Exécuter l’entraînement

Dans le notebook, modifier si besoin la variable DATA_YAML pour qu’elle pointe vers le bon fichier data.yaml.

Exemple :

DATA_YAML = r"C:chemin d'accès"

Puis exécuter les cellules d’entraînement du notebook.

Le modèle est entraîné avec YOLOv8 à partir du poids pré-entraîné yolov8n.pt.

À la fin de l’entraînement, le meilleur modèle est sauvegardé automatiquement dans :

runs/detect/train/weights/best.pt

# Évaluer le modèle

Après l’entraînement, charger le meilleur modèle :

from ultralytics import YOLO

best_model = YOLO("runs/detect/train/weights/best.pt")

Puis exécuter l’évaluation :

metrics = best_model.val(data=DATA_YAML)
metrics

Cette étape permet d’obtenir les métriques de détection : précision, rappel et mAP.

# Tester le modèle sur une image

Pour tester le modèle sur une nouvelle image :

results = best_model.predict(
    source=r"C:chemin d'accès",
    conf=0.25,
    save=True
)

L’image avec la bounding box détectée sera sauvegardée dans un dossier runs/detect/predict.

# Exécuter la décision SAFE / NOT SAFE

Le projet ne se limite pas à détecter le landing pad. Une décision d’atterrissage est prise à partir de :

la confiance de détection tau_conf

la taille relative de la bounding box tau_area

La règle utilisée est la suivante :

SAFE si confidence >= tau_conf et area_ratio >= tau_area

sinon NOT SAFE

# Lancer la démonstration webcam

Le notebook contient également une cellule permettant de lancer la webcam en temps réel.

Le programme :

détecte le landing pad

affiche la bounding box

affiche la confiance

affiche la décision SAFE / NOT SAFE

Pour arrêter l’exécution :

appuyer sur la touche Q

ou fermer la fenêtre OpenCV

# Résultat attendu

Si toutes les étapes précédentes sont suivies correctement, l’utilisateur doit pouvoir :

entraîner le modèle YOLOv8 sur le dataset fourni

évaluer les performances du modèle

tester la détection sur de nouvelles images

exécuter la démonstration webcam

obtenir une décision finale SAFE / NOT SAFE
