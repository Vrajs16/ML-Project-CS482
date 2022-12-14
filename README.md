# ML-Project-CS482

## Team Members
- Vraj Shah (vas4@njit.edu)
- Het Shah (hs737@njit.edu)

## Cvat Installation Instructions
### Mac OS Mojave
- Download docker
- Clone source code with git.
  ```shell
  git clone https://github.com/opencv/cvat
  cd cvat
  ```
- Run docker containers.
  ```shell
  docker-compose up -d
  ```
- Register a superuser.
  ```shell
  docker exec -it cvat_server bash -ic 'python3 ~/manage.py createsuperuser'
  ```
- Open chrome and login to CVAT with the credentials.
    ```shell
    http://localhost:8080
    ```
- Close docker
  ```shell
  docker-compose down
  ```

## Downloading model
run `git lfs pull` to download the model and videos

## Final Semantic Segmentation Video
[![Semantic Segmentation Video](https://img.youtube.com/vi/KvWbd59SazU/maxresdefault.jpg)](https://www.youtube.com/watch?v=KvWbd59SazU)