# Moe - Minimalist OpenGL Engine

Moe is a set of C++ classes that simplifies the creation of an OpenGL application. It provides a easy method to load shaders and textures using a TOML file provided. It allows developers to focus on creating the application rather than micro-managing each file.


## Create an OpenGL application

The example below shows how to create a small OpenGL-based app called MyApplication. First, create create *settings.toml* as shown below.

<code>settings.toml</code>
```toml
[shaders]

    # The shader program will be named shader1
    [shaders.shader1]
    vert = "vert_shader.vert"
    frag = "frag_shader.frag"


[textures]

# The texture will be named texture1
texture1 = "dirt.jpg"
```

Then, create *class MyApplication* and inherit *class Engine*. Implement the virtual functions.

<code> MyApplication.h</code>
```C++
# include "Engine.h"

class MyApplication : public moe::Engine
{
private:

    void RendorScene();            // Render the scene in each frame
    void ProcessKeyEvents();       // Implement logic for key inputs
    void ProcessMouseEvents();     // Implement logic for mouse inputs

public:
    
    void Initialize();             // Configure shaders, load models etc

};
```

<code> MyApplication.cpp</code>
```C++
void MyApplication::RendorScene() 
{
    // The base class Engine uses map and unique pointer manage shader and
    // textures. Next line shows how to use a shader program.
    shaders_.at("shader1")->Activate();

    // Similarly, to activate a texture, use the following code.
    unsigned int texture_unit = 0;
    textures_.at("texture1")->Activate(texture_unit);

    // Finally, we rendor an object that we created.
    objects_.at("object1")->Rendor();
}
```

Finally, in *main.cpp*, run *MyApplication* as such:
```C++
#include "MyApplication.h"

int main ()
{
    // some code above

    GLFWwindow* window = glfwCreateWindow(1280, 768, "Window", NULL, NULL);

    MyApplication app(window, "settings.toml");
    app.Initialize();
    app.Run();

    // some code below
}
```

It is recommended to take a look at the header files to understand what each class does.

## Dependencies 

Moe is depended on various libraries. Remember to link them in your source code.

Library | Description                                    
----------------------------------------|-------------------------------------
[GLFW](https://github.com/glfw/glfw)    | Window creation and mouse/keyboard inputs
[glad](https://github.com/Dav1dde/glad) | Modern OpenGL loader/generator
[glm](https://github.com/g-truc/glm)    | Linear algebra for computer graphics
[stb](https://github.com/nothings/stb)  | Loading image files for texture generation
[toml++](https://github.com/marzer/tomlplusplus) | Reading settings from a TOML file

Note: stb is quite a large library. Moe only uses <code>stb_image.h</code>.