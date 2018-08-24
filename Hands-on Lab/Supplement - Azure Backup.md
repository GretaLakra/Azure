![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Azure Resource Manager
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
August 2018
</div>



Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.
© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Azure Resource Manager hands-on lab step-by-step - Azure Backup](#azure-resource-manager-hands-on-lab-step-by-step---azure-backup)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Exercise 1: Create a Recovery Services vault](#exercise-1-create-a-recovery-services-vault)
        - [Task 1:](#task-1)

<!-- /TOC -->

# Azure Resource Manager hands-on lab step-by-step - Azure Backup

## Abstract and learning objectives 

In this hands-on lab, you will deploy an Azure Backup infrastructure and configure backup the SQL Servers in each environment.

-   How to deploy and configure an Azure Backup

-   How to onboard servers onto Azure Backup


## Exercise 1: Create a Recovery Services vault

Duration: 15 Minutes

In this exercise, you will configure the template to scale out to a 2nd region by deploying a 2nd instance of the architecture.  The 2nd instance will use an Application Gateway.

 > Note: Consider making a copy of the Visual Studio solution

### Task 1: 

1. Before adding the Application Gateway, delete the Load Balancer:
    In the JSON Outline window, under resources, click on the Load Balancer and delete it.

