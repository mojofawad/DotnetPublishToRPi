# DotnetPublishToRPi
Quick sample where a .NET project is created on one machine, published, then transferred to a Raspberry Pi and ran as an executable

## Prerequisites
- .NET 8.0 SDK on both the development machine and the RaspberryPi
- Docker on the RaspberryPi (if running as a container)


## Running as an executable
### Preparing for transfer
1. Pull this repository to your development machine
2. On your RaspberryPi, create a directory named `TransferHere`
3. On your development machine, open a terminal and navigate to the console project's directory `cd ./src/`
4. Run the following command to publish the project in the `PublishHere/` directory:
```bash
dotnet publish --runtime linux-arm64 --self-contined -o ../PublishHere/
```
> [!NOTE]
> Confirm that the `PublishHere` directory contains the published files. The `PublishHere` directory was specifically created above the root directory to simplify my understanding of the `publish` command

### Transferring the project
1. On your RaspberryPi, `cd` into the `TransferHere` directory
2. Run `pwd`in the `TransferHere/` directory and copy the output for step 4 below
3. On your development machine `cd ..` to the repo's root directory. You should see the `PublishHere` and `src` directories
4. Copy the `PublishHere` directory and all of its contents to the `TransferHere` directory on the RaspberryPi
```bash
# From your development machine
scp -r ./PublishHere/* <user>@<RaspberryPiAddress>:<path>
```
> [!NOTE]
> Replace `<user>` with your username and `<RaspberryPiAddress>` with the IP address of your RaspberryPi. Replace `<path>` with the output from `pwd` in step 2

> [!CAUTION]
> You will see a warning message when running the `dotnet publish` command like this:
> ```
> The "--output" option isn't supported when building a solution. Specifying a solution-level output path results in all projects copying outputs to the same directory, which can lead to inconsistent builds.`
> ```
> This example project is focused on transferring a single project to  a RaspberryPi and to simplify the understanding of the `publish` output
### Running the project
1. Run the following command in the `TransferHere/` directory to make the file executable:
```bash
chmod +x DotnetPublishSample
```
2. Run the executable:
```bash
./DotnetPublishSample
```
3. Confirm that the counter is running as expected
---
## Running as a Docker container

1. Pull this repo directly to your RaspberryPi or transfer the project to your RaspberryPi using `scp`
2. Navigate to the `src/` directory
3. Create the Docker image:
```bash
docker build -t dotnetpublishsample .
```
4. Run the Docker container:
```bash
docker run --rm dotnetpublishsample
```
5. Confirm that the counter is running as expected
6. Exit the container by pressing `Ctrl+C`