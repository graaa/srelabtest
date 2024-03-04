Hello team,

In order for this repository to work on your environment, you need to adjust the code to fit your own cloud project, let me guide you thru the files and what you need to change.

Please clone the repository for you to have the same as me.

-----CLOUD STEPS-------

-Have a working Google Cloud Account. You'll need to activate the Artifact Registry API and the kubernetes Servives one also.

-Create a repository on Artifact Registry, in there you will specify the repository path and also the region where its going to be located.

-Create an auto-pilot Cluster in Kubernetes, here the docker containers will be deployed in pods once we set the automatic image push.

-Create a Service Account in IAM setting on GCP. Grant this service account 2 important roles: Artifact Registry repository administrator and Kubernetes Engine administrator

-Go to the created Service account and access its settings, in there you will select "add key", select JSON and download it.

-----GITHUB STEPS-----

-In your repository, Go to Settings -> Secrets and Variables -> Actions. Click on "new repository secret", in there you will paste the JSON you downloaded and will name it GOOGLE_CREDENTIALS.

-Clone the Repository on your local machine with #git remote add origin <repository url>

-On resources.yaml, in line 17, adjust the images value to the folllowing: image: [your-region]-docker.pkg.dev/[your-project-id]/[your-artifact-registry-folder-name]/[docker-image-name]:latest

-On build.yaml, in line 10, change glassy-outcome-415920 for your project id, on line 22, edit the line for #gcloud auth configure-docker [your-region]-docker.pkg.dev --quiet. Also in lines 26, 27, 35 and 36 you will edit them like this:
  26.docker tag $IMAGE_NAME:latest [your-region]-docker.pkg.dev/$PROJECT_ID/[artifact-registry-repository-name]/$IMAGE_NAME:latest
  27.docker push [your-region]-docker.pkg.dev/$PROJECT_ID/[artifact-registry-repository-name]/$IMAGE_NAME:latest
  35.cluster_name: [your-k8s-cluster-name]
  36.location: [your-region]

--------------------------------------------------------------------------------------------

After all of this you should be able to run this solution. Enjoy :)
