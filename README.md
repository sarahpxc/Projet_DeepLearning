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
