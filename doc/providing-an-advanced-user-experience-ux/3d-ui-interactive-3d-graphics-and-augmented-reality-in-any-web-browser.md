The 3D-UI GE allows you to add interactive 3D elements to a web page, or
deliver a full blown 3D application on Web technology. Under the hood it
utilizes WebGL for hardware accelerated graphics rendering. However the
application developer is provided with a friendly high-level API for
describing the 3D graphics instead. You just add the 3D elements in a
so-called scene graph and the underlying rendering engine takes care of
the drawing. The GE is provided as a Javascript library to be used in
your web application.

You can either easily load existing 3D models from files or create them
with Javascript or XML embedded in the HTML page. Interactivity is
programmed in high level Javascript similarly to all Web development.

When using externally made 3D files, typically meshes for the geometry
with associated material definitions and texture images, there are
certain file formats that you can use to load them to FIWARE 3D-UI
implementations. You can use normal modelling applications such as 3D
Studio Max, Maya, Blender or Sketchup etc. to author to these formats.
There are also multiple both free and for-free model repositories from
where you can get suitable ready-made models. In 3D these files are
often called ‘assets’, and the export & import tools are an ‘asset
pipeline’ and the file collections ‘asset stores’. We document the
details of the supported asset pipelines in the GE manuals.

We provide two alternatives for this easy high level Web 3D authoring:
Three.js and XML3d.js. They are similar in purpose and scope but differ
in implementation and usage. A brief overview of both follows, with more
details in dedicated sections later and further in the manuals.

With Three.js everything is done in Javascript: you use the API to
create the scene and add the objects there. Only a single \<canvas\>
element is put to the HTML document and even that is done automatically
by the library. Three.js is the most popular 3D solution on the Web and
there are huge amounts of examples and demos made with it. It is also
used in several serious commercial services such as Microsoft’s
Photosynth2 and Sketchup’s Web viewer. Yet three.js is just a personal
project lead by the original individual developer in a very informal
manner. It’s very successful and active open source project with
contributions (mostly small and occasional) from hundreds of developers.
As a part of FIWARE we help users of this GE implementation by providing
a tested stable version, documentation and improvements to the library
where needed.

XML3D.js is the reference implementation of the XML3D specification to
add 3D support to HTML with a specific set of new tags. This way you can
add 3D elements to a web page without writing any Javascript. Similar to
the pre-existing \<img\> and \<video\> tags, \<mesh\> works to add a 3D
object to the view. The 3D elements are added to the DOM and can be
manipulated via it like other HTML elements. XML3D.js also includes an
implementation of XFlow, a new approach for declarative data processing.
The library also features advanced geometry loading to deal with huge
amounts of details (millions of polygons) with streaming and level of
detail techniques (BLAST asset bundle with the POP-buffers geometry
format). XML3D.js has been lately adopted in some projects in the
industry as well.

Finally, the WebTundra bundle integrates 3D-UI with the Synchronization
GE for creating real-time collaborative applications using WebGL
together with WebSockets. WebTundra is built using a
Model-View-Controller architecture where the rendering and networking
are separate modules built on an abstract core scene API (the model).
Currently in the 1.0 release (in FIWARE 3.3) WebTundra includes and
defaults to a Three.js view implementation. A XML3D.js view to WebTundra
was also proven feasible in an experiment. Currently WebTundra includes
an independent partial XML3D implementation: you can declare a 3D scene
view in HTML in XML3D and it gets displayed by the Three.js view.
However WebTundra does currently not populate the 3D scene to the DOM
for performance reasons (when dealing with large 3d scenes). Use of
WebTundra for networked multiuser 3d applications is also covered in a
dedicated section.

For an independent comprehensive review of 3D graphics on the net,
featuring both three.js and xml3d.js but also technologies, see:

Evans, Alun, et al. "3D Graphics on the Web: a Survey." Computers &
Graphics 41 (2014): 43-61.  
 http://www.sciencedirect.com/science/article/pii/S0097849314000260

 
