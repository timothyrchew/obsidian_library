# AWS
- installing drivers
    - following existing aws / parsec guides makes sense to get things up an running. the key part is to make sure that the IAM user has s3 access (AmazonS3ReadOnlyAccess) in order to download the GPU drivers from amazon via the parsec script. if not doing this via the parsec script, i can start fro the Nvidia gaming AMI or i can manually install the drivers via the AWS instructions for installing GPU drivers on ec2.
- gamepad
    - mac parsec will mess up my current bluetooth controller, despite my mac recognizing it fine. this can be solved by gettig one of the official bluetooth xbox gamepads which parsec explicitly supports for their mac client. 
    - easiest solution is to use native steam remote streaming so that the gamepad is recognzied within the osx client, as opposed to via parsec
- things to try:
    - can i get controls working on a mac with aws MSFS  with either steam streaming, or parsec.
    - get steam remote play working on ios. try playing 2k21 via steam remote play on ios. (https://steamcommunity.com/discussions/forum/2/3004429475624592660/)
