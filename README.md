# Cantrel SSA-03 Notes

## AWS Fundamentals

> Video 1:
> 
### Public Services
Accessed directly from an internet connection

### Private Services (AWS Private Zone)
Contained in a VPC. Can only be contacted within the VPC. Cannot be directly connected via the internet

### AWS Public Zone
Network zone where public services are operated from. Can be accessed directly from the internet. Sits between AWS Private Zone (VPC) and Public Zone (Internet)

> Video 2:

### Global Resilience
Available globally. Service can be accessed from anywhere in the world.

### Regional Resilience 
Available in the AWS Region. Example US-East-01. For Regional Resilience, you can use multiple regions (US-East-01 and US-West-01)

### Area Zone Resilience (AZ)
Available within the Area Zone. For example, VPC(s) located in a region. Example, a VPC located in US-East-01 as US-East-01-2a, US-East-01-2b and US-East-01-2c.

> Video 3:

### VPC
Virtual network inside AWS. These are regionally resilient. 

On creation, Custom VPCs are Private and Isolated unless you decide otherwise.

2 Types: 
1) Default VPC (1 per region). Initially created by AWS and preconfigured in a very specific way.
2) Custom VPCs

