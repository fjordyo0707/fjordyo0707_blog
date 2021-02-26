---
title: "Pybind_start"
date: 2021-02-26T14:22:11+08:00
draft: true
---

# Pybind
[Pybind](https://github.com/pybind/pybind11) is an open source project on Github for binding C/C++ code to python module. It is quite common that we want C/C++ speed but also python usability. Thus, we can write our core algorithm in C/C++ and then bind it into python module. 

## Binding Step
We will use [the python example](https://github.com/pybind/python_example) for the binding flow. 
The below is the manual step. We could do it step by step to get used to the binding process.

### Prepare C++ code

BindingTemplate.h
```c++
#pragma once
class BindingTemplate
{
public:
    int add(int a, int b);
};
```
BindingTemplate.cpp
```c++
#include "BindingTemplate.h"

int BindingTemplate::add(int a, int b) 
{
    return a + b;
}
```

### Prepare binding code
BindingTemplate_binding.cpp
```c++
#include "BindingTemplate.h"
#include <pybind11/pybind11.h>
#include <pybind11/operators.h>

namespace py = pybind11;

PYBIND11_MODULE(BindingTemplateModule, m) {
    m.doc() = "BindingTemplateModule";

    py::class_<BindingTemplate>(m, "BindingTemplate", py::module_local())
        .def(py::init<>())
        .def("add", &BindingTemplate::add);
}
```

### Prepare setup code
setup.py
```python
from setuptools import setup, Extension

extra_compile_args = [
    "/O2"
    ]

define_macros = [
    ('SOME_FLAG', 'None')
    ]

INCLUDE_DIRS = [
    "../source",
    "../3rdparty/pybind11/include"
    ]

LIB_DIRS = [
    "../Windows/pybind11_template/x64/Release"
    ]

LIBS = [
    "pybind11_template"
    ]

ext_modules = Extension(
    name="BindingTemplateModule",
    include_dirs=INCLUDE_DIRS,
    libraries=LIBS,
    library_dirs=LIB_DIRS,
    sources=['BindingTemplate_binding.cpp'],
    extra_compile_args=extra_compile_args,
    define_macros=define_macros,
    language='c++11')

setup(name='binding_tmeplate',
      version='0.1',
      author='Cheng-Yu Fan',
      description='This is a template code for pybind11 using python example',
      ext_modules=[ext_modules])
```


## Basic Usage
The documentation for the usage of pybind is writes in details. [First Step in pybind11](https://pybind11.readthedocs.io/en/stable/basics.html#) explain the basic usage and example.


