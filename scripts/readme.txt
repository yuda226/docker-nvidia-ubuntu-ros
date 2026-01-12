
Step 1: Commit the Container to an Image
# Syntax: docker commit <container_id_or_name> <new_image_name>
docker commit my_running_container new_image_name

Step 2: Save the Image to a File
docker save -o my_container_export.tar new_image_name

Step 3: Transfer the File

Step 4: Load the Image on the New PC
docker load -i my_container_export.tar

Step 5: Run the Container
docker run -it new_image_name bash

    # WAYLAND_DISPLAY
    #docker run -d -it --name "$CONTAINERNAME" --network host --privileged --security-opt seccomp=unconfined --cap-add=SYS_PTRACE -e DISPLAY="$DISPLAY" -e XAUTHORITY="$XAUTH" -e QT_X11_NO_MITSHM=1 -e WAYLAND_DISPLAY="$WAYLAND_DISPLAY" -e XDG_RUNTIME_DIR="/tmp" -v "$XAUTH:$XAUTH" -v /tmp/.X11-unix:/tmp/.X11-unix -v "$XDG_RUNTIME_DIR/$WAYLAND_DISPLAY:/tmp/$WAYLAND_DISPLAY" -v /etc/localtime:/etc/localtime:ro -v /dev:/dev -v /mnt:/mnt -v /media:/media -v "$(pwd)/docker_mount:/home/developer/docker_mount" new_image_name

    # NO WAYLAND_DISPLAY
    #docker run -d -it --name "$CONTAINERNAME" --network host --privileged --security-opt seccomp=unconfined --cap-add=SYS_PTRACE -e DISPLAY="$DISPLAY" -e XAUTHORITY="$XAUTH" -e QT_X11_NO_MITSHM=1 -e XDG_RUNTIME_DIR="/tmp" -v "$XAUTH:$XAUTH" -v /tmp/.X11-unix:/tmp/.X11-unix -v /etc/localtime:/etc/localtime:ro -v /dev:/dev -v /mnt:/mnt -v /media:/media -v "$(pwd)/docker_mount:/home/developer/docker_mount" new_image_name

    # Increase Shared Memory limit: shm-size=2gb
    #docker run -d -it --name "$CONTAINERNAME" --network host --privileged --security-opt seccomp=unconfined --shm-size=2gb -e DISPLAY="$DISPLAY" -e XAUTHORITY="$XAUTH" -v "$XAUTH:$XAUTH" -v /tmp/.X11-unix:/tmp/.X11-unix -v /etc/localtime:/etc/localtime:ro -v /dev:/dev -v /mnt:/mnt -v "$(pwd)/docker_mount:/home/developer/docker_mount" new_image_name