# GRPC
 1) Cloner le dépôt https://github.com/YKNB/GRPC.git sur gcloud shell 

cd GRPC 

2) Définissez le projet et la région Google Cloud et assurez-vous que l'API Google Kubernetes Engine est activée.
tapez ces commandes :  
- export PROJECT_ID=<PROJECT_ID> --> remplacer PROJECT_ID par l'id de votre projet cloud
- export REGION=us-central1
- gcloud services enable container.googleapis.com \
  --project=${PROJECT_ID}
   
 3)  Créez un cluster GKE et obtenez les informations d'identification correspondantes.

gcloud container clusters create-auto online-boutique \
  --project=${PROJECT_ID} --region=${REGION}

5) Déployer la boutique en ligne sur le cluster.

    kubectl apply -f ./release/kubernetes-manifests.yaml

7) verifier les pods :

kubectl get pods

9) Accédez au frontend web dans un navigateur en utilisant l'IP externe du frontend.

    kubectl get service frontend-external | awk '{print $4}'

    Visitez http://EXTERNAL_IP dans un navigateur web pour accéder à votre instance de Boutique en ligne.

11) Une fois que vous avez terminé, supprimez le cluster GKE.

   gcloud container clusters delete online-boutique \
  --project=${PROJECT_ID} --region=${REGION}

