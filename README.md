# Poke Snap

Poke Snap has one purpose.
```
Snap a pic
Send a poke
```

## Motivation

Social Media digitally interconnects our society at an unprecedented scale. Adding a hundred new Facebook Friends or following fifty new accounts on Instagram is a process that takes a matter of minutes to accomplish. As a Computer Science major, claiming that digitally connecting with individuals makes me feel out of touch with people must certainly be an anomaly. However, in the last three years, the only app that I've personally found fulfilling from a social dynamic perspective is Pokemon Go. In a matter of a couple of weeks in July 2016, I had befriended a dozen individuals just walking alone late at night trying to catch a Gyarados like myself. It was an app that fundamentally changed urban culture for at least a couple of months and in a way that I found personally positive.

## Purpose

Poke Snap is a Social Media app that aims to incorporate face to face interaction as a necessity. Users can 'Snap' a picture of someone's face, upload it to Poke Snap's Face Recognition Service and send the user a 'Poke'. In accordance with the purpose the app, a user will only be able to send the initial 'Poke' if they are within a *reasonable distance* of one another.

## Technical Design

### Poke Snap Services

Poke Snap currently has 4 services built to support the application.

| Name of Service | Platform Service is Running On | Architecture |
| --------------- | ------------------------------ | ------------ |
| [Poke Snap Mobile Platform](https://github.com/poke-snap/poke-snap) | [Expo](https://expo.io/) | Cross-Platform Native App |
| [Face Recognition Service](https://github.com/poke-snap/face-recognition-service) | [AWS EC2](https://aws.amazon.com/ec2/) | Microservice (Docker Container) |
| [Poke Service](https://github.com/poke-snap/poke-service) | [AWS Lambda](https://aws.amazon.com/lambda/) | Serverless (FaaS) |
| [Update Service](https://github.com/poke-snap/update-service) | [AWS Lambda](https://aws.amazon.com/lambda/) | Serverless (Faas) |

### External Services
Poke Snap also uses 2 external platforms to support the application.

| Name of Platform | Platform Type |
| ---------------- | ------------- |
| [AWS DynamoDB](https://aws.amazon.com/dynamodb/) | Key-Value Database |
| [AWS S3](https://aws.amazon.com/s3/) | Filesystem (Image Store) |


### System Overview

The overall architecture of the app and the relation between services can be seen here.
The arrows between services and platforms indicate that there are API calls made between the two.

![Poke Snap System Overview](/assets/poke_snap_design.png)

## Application Usage

### Basic Usage

| Step # | Step Explanation | Service Starting Step | Service / Platform Receiving Step |
| :----: | ---------------- | --------------------- | ------------------ |
(1) | Snap a picture of another person's face | Poke Snap Mobile Platform | N/A |
(2) | Send the picture for facial recognition | Poke Snap Mobile Platform | Face Recognition Service |
(3) | Picture has face extracted, facial recognition model determines the username | Face Recognition Service | N/A |
(4) | Username is sent to the client | Face Recognition Service | Poke Snap Mobile Platform |
(5) | Request for a poke to be sent to the user's device | Poke Snap Mobile Platform | Poke Service |
(6) | Determine if the two user (sender and user to poke) are *reasonably near* one another | Poke Service | AWS DynamoDB |
(7) | Send a poke to the user's device | Poke Service | Poke Snap Mobile Platform |

![Poke Snap Basic Usage](/assets/poke_snap_basic_usage.png)

### Background Usage

In the background usage, the Poke Snap Mobile Platform is making REST API calls to the Update Service to update the location data on AWS DynamoDB.


## License

[MIT](https://github.com/poke-snap/poke-snap.github.io/blob/master/LICENSE)
