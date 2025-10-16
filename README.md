# AzureDevops Project

##### Step 1: I created my app service:

<img width="953" height="387" alt="image" src="https://github.com/user-attachments/assets/2cb008af-2aa3-49e6-ae88-315028311cf7" />

Running website:
<img width="1098" height="898" alt="image" src="https://github.com/user-attachments/assets/34a25884-d068-487a-aa73-72a106aa0aad" />

#### Step 2: Azure P2S VPN deployment
I created a new Vnet and also GatewaySubnet as seen below:
##### Note: Ensure you use the subnet purpose specified for 'virtual network gateway'

<img width="1880" height="679" alt="image" src="https://github.com/user-attachments/assets/5b6f1be6-06eb-4953-a1d6-e39439d8c27d" />

##### Next, go to virtual network gateway and click on create 

##### Creation takes time, between 20 - 30mins....one of the longest azure service that takes long time to create

<img width="838" height="806" alt="image" src="https://github.com/user-attachments/assets/371024a9-75ea-4d96-8b5d-df253916547a" />

Afer creating, go to the created resource and configure

<img width="498" height="393" alt="image" src="https://github.com/user-attachments/assets/1380abff-0fc3-450f-9f7e-f05578f2613d" />

Check the docs, to get the details of the configuration required (Audience, Issuer)

https://learn.microsoft.com/en-us/azure/vpn-gateway/openvpn-azure-ad-tenant

<img width="474" height="379" alt="image" src="https://github.com/user-attachments/assets/74d0497d-bf7b-461d-bee3-6e4ba2287ba6" />

We need to register the Azure VPN application and grant admin consent. The user account should have cloud app admin role

Paste the URL and browse: https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent

<img width="696" height="808" alt="image" src="https://github.com/user-attachments/assets/c538c82d-3d96-4232-9716-6045546ba731" />

Click accept, and the app will be registerd in Entra ID

<img width="1904" height="606" alt="image" src="https://github.com/user-attachments/assets/693b37a7-d1cb-47b9-884b-62b7e492e4e8" />

Now, download Azure VPN client and also the VPN configuration file

<img width="830" height="574" alt="image" src="https://github.com/user-attachments/assets/6db101e0-4f2f-432b-82ec-5877c24ca403" />


On the Azure VPM client application, click + and import and save
<img width="703" height="807" alt="image" src="https://github.com/user-attachments/assets/1d32c2b1-2897-4105-bb35-5e0bd19066e4" />

It will prompt for authentication for the first time, and it should connect shortly

Now, it is connected,
<img width="490" height="678" alt="image" src="https://github.com/user-attachments/assets/3710b95a-46bf-4981-893e-9df6e98c91d1" />

Integrate App Service with a VNet

This ensures your App Service can be reached through private IP routing.

#### Step 3: In App Service → Networking → VNet Integration

- Select the same VNet as your VPN Gateway.
- Choose a dedicated subnet (not the GatewaySubnet). Hence, I quickly added 2 new subnets for the purpose of VNet and PE integration
  
<img width="1173" height="460" alt="image" src="https://github.com/user-attachments/assets/fa582e3e-4b8b-4f10-bb7e-baa38e56718a" />

#### Purpose: The App Service can now communicate privately within the VNet.

<img width="1419" height="797" alt="image" src="https://github.com/user-attachments/assets/263c27b9-6e44-4875-a10f-2dbf5f330e65" />

#### Step 4: Create a Private Endpoint for the App Service

- This gives the App Service a private IP address inside your VNet.
- Go to App Service → Networking → Private Endpoint connections → Add
- Select the same VNet and a dedicated subnet.
- Approve the connection if it shows as “Pending.”

##### Purpose: The App Service is now reachable via a private IP (no longer through the public endpoint).

<img width="1293" height="433" alt="image" src="https://github.com/user-attachments/assets/8e87dc09-92c2-41f9-9a21-04ae68a4d481" />

The DNS record:

<img width="1404" height="774" alt="image" src="https://github.com/user-attachments/assets/2449491f-d0b0-4190-a531-65e9ef28a9f1" />


#### Step 5: Disable Public Network Access

- In App Service → Networking → Access Restrictions
- Add a deny rule for all public inbound traffic.
- Ensure only the private endpoint is allowed.

✅ Purpose: Only traffic from inside your VNet can reach the App Service.

<img width="1456" height="669" alt="image" src="https://github.com/user-attachments/assets/0ff84cba-6fe2-4a38-ae67-19e0b8f83d5f" />






CHALLENGES FACED:

<img width="1899" height="769" alt="image" src="https://github.com/user-attachments/assets/48e45e47-5440-4b6a-a4c1-49204d8b10cc" />










