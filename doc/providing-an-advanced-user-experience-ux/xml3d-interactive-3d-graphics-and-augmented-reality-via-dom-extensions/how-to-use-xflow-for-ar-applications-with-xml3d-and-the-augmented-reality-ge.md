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
