Widgets can be split up into three different components:

-   Descriptor (`config.xml`), which is a declarative description of the
    widget. This file contains, among other things, references to the
    rest of resources of a widget.
-   Code, composed of HTML, JavaScript and CSS files containing the
    definition and the behaviour of the widget.
-   Static resources, such as images, documentation and other static
    resources.

The following figure shows the structure of files for a widget, which is
distributed as a zip package of such structure with `.wgt` extension.

[![How to develop new widgets and
operators](images/How-to-develop-new-widgets-and-operators.png)](images/How-to-develop-new-widgets-and-operators.png)

There are three types of operators:

-   **Data sources operators:** Operators that provide information that can
    be consumed by other widgets/operators. For example, an operator
    that retrieves some type of information from a web service.
-   **Data targets operators:** Operators that are provided information and
    use it to do some tasks. For example, an operator that receives some
    information and push it to a web service.
-   **Data transformation operators:** This type of operators can be very
    useful since they can transform data in order to make it usable by
    widgets or operators that expect data structure to be slightly
    different.

Operators use the same structure as widgets. The only difference is that
the descriptor file (`config.xml`) does not link to an initial HTML
document. Instead, it directly links to the list of javascript files.
