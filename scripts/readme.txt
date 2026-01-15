
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

    # 1. Fix X11 permissions on host
    xhost +local:docker

    # Increase Shared Memory limit: shm-size=2gb
    #docker run -d -it --name "$CONTAINERNAME" --network host --privileged --security-opt seccomp=unconfined --shm-size=2gb -e DISPLAY="$DISPLAY" -e XAUTHORITY="$XAUTH" -v "$XAUTH:$XAUTH" -v /tmp/.X11-unix:/tmp/.X11-unix -v /etc/localtime:/etc/localtime:ro -v /dev:/dev -v /mnt:/mnt -v "$(pwd)/docker_mount:/home/developer/docker_mount" new_image_name


## Inside docker container, cyclonedds related solution

    # Increase socket reciever buffer for default linux kernel limit
        # This error occurs because CycloneDDS (and ROS 2) tries to set a large buffer size for network data to prevent losing packets during high-speed 
        # commu`nication. By default, Linux has a "ceiling" (maximum limit) that is much lower than what CycloneDDS wants.
    sudo sysctl -w net.core.rmem_max=2147483647
    sudo sysctl -w net.core.wmem_max=2147483647

    # permanent update
    sudo nano /etc/sysctl.conf
    net.core.rmem_max=2147483647
    net.core.wmem_max=2147483647

    # CycloneDDS updates
    <?xml version="1.0" encoding="UTF-8" ?>
    <CycloneDDS xmlns="https://cdds.io/config">
        <Domain id="any">
            <General>
                <Interfaces>
                    <NetworkInterface name="lo"/>
                </Interfaces>
                <AllowMulticast>false</AllowMulticast>
            </General>
        </Domain>
    </CycloneDDS>

    #Look for the Middleware section. It should indicate which configuration file is being loaded. If it still fails to create a domain, it might be that the loopback interface on your new PC isn't "Up." Check it with: ip link show lo (It should say UP,LOWER_UP).
    
    ros2 doctor --report
   