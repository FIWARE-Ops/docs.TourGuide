As mentioned before, you can also offer your CKAN datasets in the FIWARE Store
by using the methods provided by the Store itself. Using the FIWARE Store
interface for offering your CKAN datasets allows you to create more advanced
offerings, including composite ones, offerings with multiple pricing models,
offerings with recurring and usage pricing models, etc. As an alternative, you
can use the CKAN interface to create a basic offering, and then use the FIWARE
Store interface to include these advanced features.

To create offerings, the first step is entering the
[FIWARE Store](https://store.lab.fiware.org/) and signing in using a valid
FIWARE Account. Once in the FIWARE Store, click on the “My Stock” button in the
left side bar in order to enter to the sellers section.

[![Offering Datasets Through the FIWARE Store
Interface](../images/Offering-Datasets-Through-the-WStore-Interface.png)](../images/Offering-Datasets-Through-the-WStore-Interface.png)

Offering a dataset in the FIWARE Store requires the creation of two different
entities. On the one hand, you have to create a Product Specification which
represents the dataset itself and contains the technical information such as its
URL. On the other hand, you have to create a Product Offering which binds the
Product Specification with a pricing model. Note that you can include a Product
Specification in multiple offerings.

To register your dataset as a Product Specification, click on “Product
Specifications” and then on the “New” button.

[![Offering Datasets Through the FIWARE Store
Interface1](../images/Offering-Datasets-Through-the-WStore-Interface1.png)](../images/Offering-Datasets-Through-the-WStore-Interface1.png)

In this view, it is displayed a form distributed in several steps each intended
to cover specific information of the Product Specification. Following it is
described the purpose of these steps and the information you have to provide
(Only the steps required for creating a CKAN Dataset are described, for more
details have a look at the user guide in
[here](https://business-api-ecosystem.readthedocs.io/en/latest/user-guide.html)):

-   **General**: This step is intended to provide the basic information of the
    product specification. being created. In particular, you have to fill in the
    following fields:

    -   Name: The name of your dataset (it does not have to match with the name
        provided in CKAN).
    -   Version: The version of the dataset (typically: 1.0)
    -   Brand: Your brand name (It migth be your username if you do not have
        one)
    -   ID Number: An external ID used for identify the dataset outside of the
        FIWARE Store (i.e the ID in CKAN of the dataset)
    -   Description: Description of the dataset

[![Offering Datasets Through the FIWARE Store
Interface2](../images/Offering-Datasets-Through-the-WStore-Interface2.png)](../images/Offering-Datasets-Through-the-WStore-Interface2.png)

-   **Assets**: This step is intended to provide information of the asset to be
    included as a product specification. In this case, as the CKAN dataset is a
    digital product, you have to set the “Is Digital product” checkbox to yes.
    For including the dataset info, you have to fill the following fields:

    -   Digital Asset Type: Type of the asset to be provided, selected among the
        ones supported by the FIWARE Store. In this case, you have to select
        “CKAN Dataset”
    -   Asset URL: the URL that you get when you create your dataset in the
        FIWARE Data Portal (typically:
        https://data.lab.fiware.org/dataset/{dataset_name}). You can check this
        URL by accessing your dataset in FIWARE Data Portal.
    -   Media Type: MIME type of the dataset (e.g application/json, text/csv,
        etc.)

[![Offering Datasets Through the FIWARE Store
Interface3](../images/Offering-Datasets-Through-the-WStore-Interface3.png)](../images/Offering-Datasets-Through-the-WStore-Interface3.png)

-   **Attachments**: This step is intended to provide the picture to be shown in
    the Store portal regarding the product specification. To provide the picture
    fill in the following fields:

    -   How to provide: Whether you want to provide a URL of the picture or
        upload it directly to the platform
    -   Picture URL: URL of the picture if you have decided to use a URL
    -   Upload Picture: File field to upload the image if you have decided to
        provide it as a file

[![Offering Datasets Through the FIWARE Store
Interface4](../images/Offering-Datasets-Through-the-WStore-Interface4.png)](../images/Offering-Datasets-Through-the-WStore-Interface4.png)

-   **Terms & Conditions**: This step is used to include the terms and
    conditions that apply to the Product Spec and that must be accepted by the
    customers. To provide the terms and conditions fill the following fields:

    -   Agreement title: Title given to the terms and conditions clauses. Note
        that it matches the CKAN “License” field when registering a CKAN dataset
    -   Agreement text: Text with the terms and conditions clauses. Note that it
        matches the CKAN “License Description” field when registering a CKAN
        dataset

[![Offering Datasets Through the FIWARE Store
Interface5](../images/Offering-Datasets-Through-the-WStore-Interface5.png)](../images/Offering-Datasets-Through-the-WStore-Interface5.png)

When you have registered your dataset as a Product Specification, the next step
in order to make it available to the FIWARE Store customers, is including it in
a Product Offering. To do so, go to the “Offerings” section and then click on
the “New” button.

[![Offering Datasets Through the FIWARE Store
Interface6](../images/Offering-Datasets-Through-the-WStore-Interface6.png)](../images/Offering-Datasets-Through-the-WStore-Interface6.png)

As you can see, a new form is displayed which will guide you throughout the
process of creating a new Product Offering. In particular, the steps you have to
follow in order to create an offering for your dataset Product Spec are the
following:

-   **General**: This step is intended to provide the basic info of the Product
    Offering being created. Specifically, You have to fill the following fields:

    -   Name: The name you want to give to your Product Offering.
    -   Version: The current version of your Product Offering
    -   Description: The textual description of your Product Offering that will
        be displayed to the FIWARE Store customers
    -   Places: List of places where you offering will be available

[![Offering Datasets Through the FIWARE Store
Interface7](../images/Offering-Datasets-Through-the-WStore-Interface7.png)](../images/Offering-Datasets-Through-the-WStore-Interface7.png)

-   **Product Spec**: This step is intended to specify the Product Spec to be
    included in the Product Offering. In this step you have to select the
    dataset product specification you created previously.

[![Offering Datasets Through the FIWARE Store
Interface8](../images/Offering-Datasets-Through-the-WStore-Interface8.png)](../images/Offering-Datasets-Through-the-WStore-Interface8.png)

-   **Catalogue**: This step is used to specify the Catalogue where you want to
    publish the Product Offering being created

[![Offering Datasets Through the FIWARE Store
Interface9](../images/Offering-Datasets-Through-the-WStore-Interface9.png)](../images/Offering-Datasets-Through-the-WStore-Interface9.png)

-   **Category**: This step is used to specify a set of categories for your
    Product Offering in order to easy the process of finding it to potential
    customers

[![Offering Datasets Through the FIWARE Store
Interface10](../images/Offering-Datasets-Through-the-WStore-Interface10.png)](../images/Offering-Datasets-Through-the-WStore-Interface10.png)

-   **Price Plans**: This step is intended to create the Pricing model of your
    Product Offering. You can create different price plans which can be selected
    by the customers according to their needs. To create a price plan click on
    “New price plan”. Then, you have to fill the displayed form and include it
    in the pricing model clicking on “Create”. The different fields you have to
    fill in order to create a price plan are described below:

    -   Name: The name of the price plan used to identify it within the pricing
        model.
    -   Type: The type of the price plan. It can be “One Time”, “Recurring”, or
        “Usage”
    -   Price: The amount to be charged: once for one time models, periodically
        for recurring models, and based on the consumption for usage models
    -   Description: The description of the price plan
    -   Charge period: This field is only used in recurring plans and contains
        the period between charges
    -   Unit: This field is only used in usage plans and contains the unit to be
        monitored in order to calculate the charges

[![Offering Datasets Through the FIWARE Store
Interface11](../images/Offering-Datasets-Through-the-WStore-Interface11.png)](../images/Offering-Datasets-Through-the-WStore-Interface11.png)

-   **RS Model**: This step is intended to specify the Revenue Sharing model
    used to distribute the revenue generated by the offering

[![Offering Datasets Through the FIWARE Store
Interface12](../images/Offering-Datasets-Through-the-WStore-Interface12.png)](../images/Offering-Datasets-Through-the-WStore-Interface12.png)

At this point, the Product Offering is already created but is not published yet.
To publish the offering, it is needed to update the status of the Product Spec
and the Product Offering to “Launched”. To do that, open the details of the
Product Spec or the Product Offering, click on “Launched” and then on “Update”.

[![Offering Datasets Through the FIWARE Store
Interface13](../images/Offering-Datasets-Through-the-WStore-Interface13.png)](../images/Offering-Datasets-Through-the-WStore-Interface13.png)

[![Offering Datasets Through the FIWARE Store
Interface14](../images/Offering-Datasets-Through-the-WStore-Interface14.png)](../images/Offering-Datasets-Through-the-WStore-Interface14.png)
