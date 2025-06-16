# GCP Enterprise Networking: Building a Hub-and-Spoke Model with Network Connectivity Center

## Project Overview
This project demonstrates the implementation of a scalable, centralized network topology on Google Cloud Platform (GCP). The architecture connects multiple isolated Virtual Private Cloud (VPC) networks through a central hub using the Network Connectivity Center (NCC). This model is fundamental for organizations that need to centralize shared services (like logging, monitoring, or security inspection) while providing network segmentation for different teams, environments, or applications.

Initially, two separate "spoke" VPCs were created, representing distinct business units or application environments. Connectivity tests confirmed these networks were completely isolated, unable to communicate with each other, as intended for security and operational separation.

By configuring a Network Connectivity Center hub and attaching each VPC as a spoke, I established transitive routing between them. This allows the spokes to communicate with each other through the central hub, simplifying network management and creating a scalable foundation for future growth.

## Real-World Scenario
Imagine a company with separate VPCs for its development (spoke1-vpc) and quality assurance (spoke2-vpc) teams. 
For security, these networks are isolated. However, both teams need access to a shared set of tools, such as a CI/CD server or an artifact repository, located in a central shared services (hub-vpc) network.

Instead of creating complex and hard-to-manage VPC peerings between every network, the Hub-and-Spoke model centralizes connectivity. The Network Connectivity Center acts as the managed backbone, ensuring that all inter-VPC traffic is routed through the central hub.

## What I Built & Key Steps
1. Initial State Verification:
Deployed three distinct VPCs: hub-vpc, spoke1-vpc, and spoke2-vpc.
Provisioned a Compute Engine VM in each VPC (hub-vm, spoke1-vm, spoke2-vm).
Confirmed complete network isolation by running ping tests between spoke1-vm and spoke2-vm, which resulted in 100% packet loss as expected.

2. Hub-and-Spoke Implementation:
Created a Network Connectivity Center Hub named my-hub to serve as the central point for inter-VPC connectivity.
Attached spoke1-vpc and spoke2-vpc to the hub as VPC Spokes. This automatically exchanges routes between the networks via the hub.

3. Final State Verification:
Retested connectivity by successfully pinging the internal IP address of spoke2-vm from spoke1-vm. This confirmed that the hub was correctly routing traffic between the two spoke networks.
Utilized the Network Topology tool to visualize the new network architecture, observing the flow of traffic between the connected entities.

## Security & Networking Principles Demonstrated
Network Segmentation: The initial setup reflects a best practice of isolating environments in their own VPCs to limit the blast radius of any potential security incident.
Centralized Connectivity: Using NCC avoids a complex "full mesh" of VPC peerings, which is difficult to manage and scale. This hub-and-spoke model simplifies administration and firewall rule management.

Transitive Routing: Network Connectivity Center provides a managed and scalable solution for transitive routing, a capability not offered by standard VPC Peering.
Infrastructure as a Foundation: This architecture provides a central point (the hub) where security services like Cloud IDS (Intrusion Detection System) or third-party firewall appliances can be deployed to inspect all inter-spoke traffic.

## Key Skills & Technologies Used
A. Networking: Google Cloud VPC, Subnets, Firewall Rules, Hub-and-Spoke Architecture

B. Compute: Google Compute Engine (GCE)

C. Connectivity: Network Connectivity Center (NCC)

D. Monitoring: Network Topology Visualization

E. CLI & Diagnostics: gcloud, ping for connectivity testing

F. Potential Enhancements

This foundational project could be extended by:
Automating with Terraform: Writing Infrastructure as Code (IaC) to deploy the entire hub-and-spoke architecture automatically.

Implementing a Centralized Firewall: Deploying a next-generation firewall (NGFW) appliance in the hub-vpc to inspect all traffic flowing between the spokes.

Hybrid Connectivity: Adding a VPN tunnel or Cloud Interconnect attachment to the hub to connect an on-premises data center to the cloud network.
