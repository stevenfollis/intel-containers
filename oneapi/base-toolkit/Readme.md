docker build --tag stevenfollis/oneapi:base-toolkit-2024.2.1 .

docker run \
    --env GRANT_SUDO=yes \
    --interactive \
    --name jhub \
    --publish 8888:8888 \
    --rm \
    --tty \
    --user root \
    jhub:latest \
    bash