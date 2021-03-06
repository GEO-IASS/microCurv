microCurv - The image curvature microscope
by Pascal Monasse <monasse@imagine.enpc.fr>,
   Adina Ciomaga <adina@math.uchicago.edu>
   Lionel Moisan <Lionel.Moisan@parisdescartes.fr>

This software computes the mean curvature map of an image. It is linked to an IPOL workshop [1]. This is based on level line tree extraction of the bilinear interpolation of a digital image and affine invariant smoothing. These functionalities were inspired from the equivalent MegaWave2 [2] functions 'flst_bilinear' and 'gass', though the tree extraction is based on a much simpler algorithm than 'flst_bilinear'.

[1] IPOL workshop, http://dev.ipol.im/~monasse/ipol_demo/cmmm_image_curvature_microscope/
[2] MegaWave2, http://megawave.cmla.ens-cachan.fr/

Version 0.1 released on 2014/12/23

Future releases and updates:
https://github.com/pmonasse/microCurv.git

Build
-----
Prerequisites: CMake version 2.6 or later, libTIFF, libPNG

- Unix, MacOS, Windows with MinGW:
$ cd /path_to_this_file/
$ mkdir Build && cd Build && cmake -DCMAKE_BUILD_TYPE:bool=Release ..
$ make

CMake tries to find libPNG and libTIFF on your system. They need to come with header files, which are provided by "...-dev" packages under Linux Debian or Ubuntu.

- Test:
$ ./microCurv ../data/flower.png out.png
Compare out.png with ../data/flowerOut.png

Usage
-----
microCurv [options] in.png outCurv.png [outCurv.tif]
  - in.png: PNG input image
  - outCurv.png: PNG output image with superimposed curvatures
  - outCurv.tif: float TIFF output image of mean curvature map
Options:
  -q <int>: quantization level step of level lines (default 8)
  -s <float>: scale of smoothing by affine scale-space (default 2)
  -o <fileName.png>: output image after level line smoothing
  -I <fileName>: output file for level lines before smoothing
  -O <fileName>: output file for level lines after smoothing
The extension of <fileName> in -I and -O options determines the file format: SVG (Scalable Vector Graphics, extension .svg) or PNG (any other extension).

Two additional programs are built:
- extractLines [-p|--precision prec] [-o|--offset o] [-s|--step s] [-r|--reconstruct out.png] [-l lastScale] im.png lines.txt
- smoothLines  [-l lastScale] flistIn flistOut
The txt output file of extractLines can be used by MegaWave2 [2]:
$ flreadasc 2 out.flists < lines.txt && fkview out.flists
The input and output files of smoothLines use the same text format.

Files
-----
CMakeLists.txt
cmdLine.h
curv.cpp
curv.h
draw_curve.cpp
draw_curve.h
fill_curve.cpp
fill_curve.h
gass.cpp
gass.h
image.cpp
image.h
io_png.c
io_png.h
io_tiff.c
io_tiff.h
levelLine.cpp
levelLine.h
LICENSE.txt
lltree.cpp
lltree.h
main_extraction.cpp
main_gass.cpp
microCurv.cpp
README.txt
usage.txt
xmtime.h
data/flower.png
data/flowerOut.png

Limitations
-----------
The program extracts bilinear level lines at half-integer values (0.5, 1.5, etc). This can be modified (constant 'offset'), but it is important that the quantization avoids initial image levels (since iso-level sets can be quite complex at these levels).
It is mandatory that all level lines are closed. For this, put a constant level along the image boundary. The programs put a 0 gray level. Calling the function 'extract' (which LLTree constructor does) without satisfying this requirement exposes to a crash.
