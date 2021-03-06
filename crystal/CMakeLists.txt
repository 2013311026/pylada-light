###############################
#  This file is part of PyLaDa.
#
#  Copyright (C) 2013 National Renewable Energy Lab
#
#  PyLaDa is a high throughput computational platform for Physics. It aims to
#  make it easier to submit large numbers of jobs on supercomputers. It
#  provides a python interface to physical input, such as crystal structures,
#  as well as to a number of DFT (VASP, CRYSTAL) and atomic potential
#  programs. It is able to organise and launch computational jobs on PBS and
#  SLURM.
#
#  PyLaDa is free software: you can redistribute it and/or modify it under the
#  terms of the GNU General Public License as published by the Free Software
#  Foundation, either version 3 of the License, or (at your option) any later
#  version.
#
#  PyLaDa is distributed in the hope that it will be useful, but WITHOUT ANY
#  WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
#  FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more
#  details.
#
#  You should have received a copy of the GNU General Public License along
#  with PyLaDa.  If not, see <http://www.gnu.org/licenses/>.
###############################

add_library(cutilities STATIC smith_normal_form.cc gruber.cc noopt.cc)
set_property(TARGET cutilities PROPERTY CXX_STANDARD 11)
if(CMAKE_SYSTEM_PROCESSOR STREQUAL "x86_64")
  set_target_properties(cutilities PROPERTIES COMPILE_FLAGS "-fPIC")
endif()
add_python_module("pylada.crystal" CPP GLOB *.py cutilities.pyx _space_group.pyx _primitive.pyx
  _coordination_shells.pyx _map_sites.pyx)
target_link_libraries(pylada.crystal.cutilities-cython cutilities)
if(TARGET Lookup-Eigen3)
  add_dependencies(cutilities Lookup-Eigen3)
endif()

add_subdirectory(defects)
add_subdirectory(tests)
