Primero crearemos el script a ejecutar (hola.py) 

~~~
from pytube import YouTube

#link = input("Enter the link: ")
yt = YouTube("https://www.youtube.com/watch?v=EnbfIcQKROU")

#Title of video
print("Title: ",yt.title)
#Number of views of video
print("Number of views: ",yt.views)
#Length of the video
print("Length of video: ",yt.length,"seconds")
#Description of video
print("Description: ",yt.description)
#Rating
print("Ratings: ",yt.rating)

yt.streams.filter(progressive=True, file_extension='mp4').order_by('resolution').desc().first().download()
~~~


Lo siguiente sera crear el Dockerfile y el requirements.txt

~~~
FROM python:3

WORKDIR /usr/src/app

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY ./app /usr/src/app

CMD [ "python", "./hola.py" ]
~~~

~~~
pytube==12.1.2
PyChromecast
~~~
lo siguiente sera crear una imagen usando el siguiente comando
~~~
sudo docker build -t youtubeimagen:version . 
~~~
el punto es importante

---
lo siguiente que haremos sera un docker-compose.yml para no tener que memorizar el comando del docker run 
~~~
 services:
    python:
        image: youtubeimagen:version
        volumes:
            - ./app:/usr/src/app
        stdin_open: true
        tty: true
        working_dir: /usr/src/app
        command: python hola.py

~~~

Ahora haremos un docker-compose up y deberia descargar el video del script

---
Lo siguiente sera subir la imagen a docker hub

para ello haremos lo siguiente
lo primero es crear un repositorio en github con el nombre que queramos, nos dara despues de ponerle nombre iremos al Dockerfile y haremos nuevamente un docker build con el nombre que le hayamos puesto al repositorio online, por ejemplo si el repositorio es samusakamoto/youtubeimagen tendremos que ponerle en el docker build el nombre samusakamoto/youtubeimagen:latest, ahora para subirlo lo que haremos sera un docker login y luego pondremos el siguiente comando
~~~
docker push samusakamoto/youtubeimagen:latest
~~~

y con eso estaria subido