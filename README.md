### How to Build an Alpine Linux Package (https://www.matthewparris.org/build-an-alpine-package/)

#### Generate the Checksums
docker run --rm -it -v "$PWD":/home/builder/package andyshinn/alpine-abuild:v15 checksum

### Keys
docker run --name keys --entrypoint abuild-keygen -e PACKAGER="John Doe <john@doe.com>" andyshinn/alpine-abuild:v15 -n

docker cp keys:/home/builder/.abuild/john@doe.com-632d3312.rsa /home/dev/alpine-package/ </br>
docker cp keys:/home/builder/.abuild/john@doe.com-632d3312.rsa.pub /home/dev/alpine-package/ </br>

docker rm -f keys

### Build It!
docker run --rm -it \
	-e RSA_PRIVATE_KEY="$(cat /home/dev/alpine-package/john@doe.com-632d3312.rsa)" \
	-e RSA_PRIVATE_KEY_NAME="john@doe.com-632d3312.rsa" \
	-v "$PWD:/home/builder/package" \
	-v "/home/dev/alpine-package/packages:/packages" \
    -v "/home/dev/alpine-package/john@doe.com-632d3312.rsa.pub:/etc/apk/keys/john@doe.com-632d3312.rsa.pub" \
	andyshinn/alpine-abuild:v15

### Add a local package
docker cp /home/dev/alpine-package/packages/builder/x86_64/sway-aither-1.7-r4.apk alpine-container-name:/root

apk add --allow-untrusted /root/sway-aither-1.7-r4.apk
