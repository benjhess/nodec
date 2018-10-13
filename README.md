# node packer - docker build environment
Creates a docker image, that contains a build environment for packing node.js applications into a single executable. Contains precompiled node sources. This way the build process is a lot faster!


# How to
If you forked this, rename property **"user"** in package.json to your docker hub username.

### Build the image

    npm run build

### Run test container from image

    npm run container

### Push the image to docker hub

    npm run push

### Delete the image

    npm run delete

# Example: packing node app into binary
This example creates a stand alone binary including node and the app sources.

    npm run container
    
    mkdir example
    echo "console.log('hello world');" >  example/hello-world.js
    
    # creates binary "hello-world" from "example/hello-world.js"
    nodec -r example/ --skip-npm-install -o hello-world example/hello-world.js
    
    # shrinks binary (36M > 12M)
    upx hello-world



