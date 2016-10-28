As mentioned before, you can also publish the datasets in the Store by
using the methods provided by the Store itself. To do so, first of all
you have to enter the [FIWARE
Store](https://store.lab.fiware.org/) and sign in. Once in the Store,
you should click in the “My Stock” button, in the left side bar. 

[![Offering Datasets Through the WStore
Interface](../images/Offering-Datasets-Through-the-WStore-Interface-1024x326.png)](../images/Offering-Datasets-Through-the-WStore-Interface.png)

To register your dataset as a product, click on “Product Specifications”
and then in the “New” button.

[![Offering Datasets Through the WStore
Interface1](../images/Offering-Datasets-Through-the-WStore-Interface1.png)](../images/Offering-Datasets-Through-the-WStore-Interface1.png)

In the new form you will have to complete some fields, ditributed in several steps (Only the steps required for creating a CKAN Dataset are described):

* **General**: General information of the product to be created. You have to fill the following fields:

    -   Name: The name of your dataset (it doesn’t have to match with the name
        provided in CKAN).
    -   Version: The version of the dataset (typically: 1.0)
    -   Brand: Your brand name (It migth be your username if you don’t have one)
    -   ID Number: An external Id used for identify the dataset outside of the FIWARE Store (i.e the Id in CKAN of the dataset)
    -   Description: Description of the dataset

[![Offering Datasets Through the WStore
Interface2](../images/Offering-Datasets-Through-the-WStore-Interface2.png)](../images/Offering-Datasets-Through-the-WStore-Interface2.png)

* **Assets**: Information of the asset to be included as a product. In this case, as the CKAN dataset is a digital product, you have to set the  “Is Digital product” checkbox to yes. For providing the dataset you have to fill the following fields:

    -   Digital Asset Type: Type of the asset to be provided. You have to select “CKAN Dataset”
    -   Asset URL: the URL that you get when you create your dataset in
        the FIWARE Data Portal (typically:
        https://data.lab.fiware.org/dataset/{dataset\_name}). You can check
        this URL by accessing your dataset in FIWARE Data Portal.
    -   Media Type: Mime type of the dataset (e.g application/json, text/csv, etc.)


[![Offering Datasets Through the WStore
Interface3](../images/Offering-Datasets-Through-the-WStore-Interface3.png)](../images/Offering-Datasets-Through-the-WStore-Interface3.png)

* **Attachments**: Picture to be shown in the Store portal regarding the product. To provide the picture fill the following fields:

    -   How to provide: Whether you want to provide a URL of the picture or upload it directly to the platform
    -   Picture URL: URL of the picture if you have decided to use a URL
    -   Upload Picture: File field to upload the image if you have decided to provide it as a file

[![Offering Datasets Through the WStore
Interface4](../images/Offering-Datasets-Through-the-WStore-Interface4.png)](../images/Offering-Datasets-Through-the-WStore-Interface4.png)

* **Terms & Conditions**: Terms and conditions that apply to the product and that must be accepted by the customers.

    -   Agreement title: Title given to the terms and conditions clauses
    -   Agreement text: Text with the terms and conditions clauses

[![Offering Datasets Through the WStore
Interface5](../images/Offering-Datasets-Through-the-WStore-Interface5.png)](../images/Offering-Datasets-Through-the-WStore-Interface5.png)

When you have registered your dataset as a product, you will be able to
offer it in the Store. To do so, you have to create an offering. You
can do it by going to the “Offerings” section and then click in the “New”
button.

[![Offering Datasets Through the WStore
Interface6](../images/Offering-Datasets-Through-the-WStore-Interface6.png)](../images/Offering-Datasets-Through-the-WStore-Interface6.png)

A new form will be displayed. This form will guide throughout the
process of creating a new offering. In particular, the steps you have
to follow in order to create an offering for your dataset product are
the following:

* **General**: General information regarding the offering. You have to fill the following fields:

    -   Name: The name of your offering
    -   Version: The version of your offering
    -   Description: The description of your offering
    -   Places: List of places where you offering will be available

[![Offering Datasets Through the WStore
Interface7](../images/Offering-Datasets-Through-the-WStore-Interface7.png)](../images/Offering-Datasets-Through-the-WStore-Interface7.png)

* **Product Spec**: Product to be included in the offering. In this step you have to select your dataset product.

[![Offering Datasets Through the WStore
Interface8](../images/Offering-Datasets-Through-the-WStore-Interface8.png)](../images/Offering-Datasets-Through-the-WStore-Interface8.png)

* **Catalogue**: Catalogue where you want to publish the offering

[![Offering Datasets Through the WStore
Interface9](../images/Offering-Datasets-Through-the-WStore-Interface9.png)](../images/Offering-Datasets-Through-the-WStore-Interface9.png)

* **Category**: Set of categories of your offering used to easy the process of finding it to potential customers

[![Offering Datasets Through the WStore
Interface10](../images/Offering-Datasets-Through-the-WStore-Interface10.png)](../images/Offering-Datasets-Through-the-WStore-Interface10.png)

* **Price Plans**: Pricing model of your offering. You can create different price plans which can be selected by the customers according to their needs. You can create price plans clicking in “New price plan”. Then, you have to fill the form and click in “Create” to save the plan. To create a plan you have to provide the following information:

    -   Name: The name of the price plan
    -   Type: The type of the price plan. It can be “One Time”, “Recurring”, or “Usage”
    -   Price: The amount to be charged once for one time plans, periodically for recurring models, and based on the consumption for usage models
    -   Description: The description of the price plan
    -   Charge period: This field is only used in recurring plans and contain the period between charges
    -   Unit: This field is only used in usage models and contains the unit to be monitored in order to calculate the charges

[![Offering Datasets Through the WStore
Interface11](../images/Offering-Datasets-Through-the-WStore-Interface11.png)](../images/Offering-Datasets-Through-the-WStore-Interface11.png)

* **RS Model**: Revenue Sharing model used to distribute the revenue generated by the offering

[![Offering Datasets Through the WStore
Interface12](../images/Offering-Datasets-Through-the-WStore-Interface12.png)](../images/Offering-Datasets-Through-the-WStore-Interface12.png)


At this point, the offering is already created but is not published yet.
To publish the offering, we should update the status of the product and
the offering to  “Launched”. To do that, open the details of the product or
the offering, click in “Launched” and then in “Update” 


[![Offering Datasets Through the WStore
Interface13](../images/Offering-Datasets-Through-the-WStore-Interface13.png)](../images/Offering-Datasets-Through-the-WStore-Interface13.png)

[![Offering Datasets Through the WStore
Interface14](../images/Offering-Datasets-Through-the-WStore-Interface14.png)](../images/Offering-Datasets-Through-the-WStore-Interface14.png)
