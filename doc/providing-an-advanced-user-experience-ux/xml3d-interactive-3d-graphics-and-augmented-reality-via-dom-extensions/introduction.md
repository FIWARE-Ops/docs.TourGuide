XML3D is an extension to HTML5 for declarative 3D content represented as
a scene graph like structure inside the DOM. All nodes within this graph
are also nodes in the web sites DOM tree representation and can be
accessed and changed via JavaScript like any other common DOM elements
as well. On these DOM nodes, HTML events can be registered similar to
known HTML elements. Resources for mesh data can be stored externally in
any kind of external format (e.g. JSON, XML or binary) and referenced by
URL.

This allows assembling your 3D application easily and directly as part
of your website. 3D-UI-XML3D comes with a large set of examples to show
its basic usage. XML3D is implemented as polyfill implementation and
provided in a single JavaScript file that, when linked to the website,
provides access to the full feature set of XML3D.

With XML3D comes Xflow, a declarative data flow processing approach that
allows complex computations on your 3D scene. Xflow is directly
implemented in xml3d.js and highly optimized towards operations on XML3D
scenes. Applications range from computing object transformations in key
frame animations to skinning of 3D models for skeletal animations.

Outside the application field of 3D scene modification, Xflow is also a
great tool to perform sophisticated image processing tasks.

**How to set up a first XML3D scene**

A set of example applications is deployed with your instance of XML3D
when you setup your instance of XML3D via the FI-Lab recipe. The same
set of example is also available online in the xml3d-examples github
repository:  
 http://xml3d.github.io/xml3d-examples/

Setting up a 3D scene with XML3D is simple. All you need is a web server
to which the website can be deployed. If you install the
3D-UI-XML3D-GEri directly from the recipe, an instance of an Apache2
server is already deployed.

To enable XML3D in your website, just add a link to the xml3d.js script
file to your website’s HTML.

A detailed tutorial of how to set up 3D scenes is given in the XML3D
eLearning courses and the 3D-UI-XML3D-GEi User and Programmer’s guide.

**How to use Xflow for AR applications with XML3D and the Augmented
Reality GE**

Xflow, which is part of xml3d.js, can not only be used to modify 3D
scenes based for example on vertex data. It can also be employed to
perform image processing computations, both on pixel data of static
images and on data of video streams.  
 The latter builds the foundation for marker tracking in Augmented
Reality (AR) applications.

Like for the 3D scene examples, a set of examples for image processing
is deployed with your instance of the 3D-UI-XML3D GE right away. This
includes example applications that overlay a video stream with an XML3D
object. For the application, it makes no difference whether the image
data comes from a pre-computed video, or from an actual webcam video
stream.

The use-case of Xflow for marker tracking in video streams is
implemented in the Augmented Reality Generic Enabler. This Generic
Enabler provides a simple interface to define markers that should be
tracked and to change camera or object transformations based on a
tracked marker in the scene.

 

 
