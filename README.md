# ImGuiMarkdown

## Markdown for Dear ImGui

A permissively licensed markdown single-header library for [Dear ImGui](https://github.com/ocornut/imgui).

ImGuiMarkdown currently supports the following markdown functionality:

  * Wrapped text
  * Headers H1, H2, H3
  * Indented text, multi levels
  * Unorderd lists and sub-lists
  * Links

![ImGuiMarkdown demo live editing](
https://github.com/juliettef/Media/blob/master/ImGuiMarkdown_demo_live_editing.gif)

## Example use on Windows with links opening in a browser

```Cpp

#include "ImGui.h"
#include "ImGuiMarkdown.h"

// Following includes for Windows LinkCallback
#define WIN32_LEAN_AND_MEAN
#include <Windows.h>
#include "Shellapi.h"

void LinkCallback( const char* link_, uint32_t linkLength_ )
{
    std::string url( link_, linkLength_ );
    ShellExecuteA( NULL, "open", url.c_str(), NULL, NULL, SW_SHOWNORMAL );
}

void LoadFonts( float fontSize_ = 12.0f )
{
    ImGuiIO& io = ImGui::GetIO();
    io.Fonts->Clear();
    // Base font, font index = 0
    io.Fonts->AddFontFromFileTTF( "myfont.ttf", fontSize_ );
    // Bold headings H2 and H3, font index = 1
    io.Fonts->AddFontFromFileTTF( "myfont-bold.ttf", fontSize_ );
    // bold heading H1, font index = 2
    float fontSizeH1 = fontSize_ * 1.1f;
    io.Fonts->AddFontFromFileTTF( "myfont-bold.ttf", fontSizeH1 );
}

// You can make your own RenderMarkdown function with your prefered string container and markdown config.
void RenderMarkdown( const std::string& markdown_ )
{
    // fonts for, respectively, headings H1, H2, H3 and beyond
    ImGui::MarkdownConfig mdConfig{ LinkCallback, { 2, true, 1, true, 1, false } };
    ImGui::RenderMarkdown( markdown_.c_str(), markdown_.length(), mdConfig );
}

void RenderMarkdownExample()
{
    const std::string markdownText = u8R"(
# H1 Header: Text and Links
You can add [links like this one to enkisoftware](https://www.enkisoftware.com/) and lines will wrap well.
## H2 Header: indented text.
  This text has an indent (two leading spaces).
    This one has two.
### H3 Header: Lists
  * Unordered lists
    * Lists can be indented with two extra spaces.
  * Lists can have [links like this one to Avoyd](https://www.avoyd.com/)
)";
    RenderMarkdown( markdownText );
}
```

![Example use of ImGuiMarkdown with icon fonts](https://github.com/juliettef/Media/blob/master/ImGuiMarkdown_icon_font.jpg)

## Projects using ImGuiMarkdown

### [Avoyd](https://www.avoyd.com)
Avoyd is an abstract 6 degrees of freedom voxel game. The game and the voxel editor's help and tutorials use ImGuiMarkdown with Dear ImGui.

## Credits

Design and implementation - [Doug Binks](http://www.enkisoftware.com/about.html#doug) - [@dougbinks](https://github.com/dougbinks)  
Implementation and maintenance - [Juliette Foucaut](http://www.enkisoftware.com/about.html#juliette) - [@juliettef](https://github.com/juliettef)  
Thanks to [Omar Cornut for Dear ImGui](https://github.com/ocornut/imgui).

## License (zlib)

Copyright (c) 2019 Juliette Foucaut and Doug Binks

This software is provided 'as-is', without any express or implied
warranty. In no event will the authors be held liable for any damages
arising from the use of this software.

Permission is granted to anyone to use this software for any purpose,
including commercial applications, and to alter it and redistribute it
freely, subject to the following restrictions:

1. The origin of this software must not be misrepresented; you must not
   claim that you wrote the original software. If you use this software
   in a product, an acknowledgement in the product documentation would be
   appreciated but is not required.
2. Altered source versions must be plainly marked as such, and must not be
   misrepresented as being the original software.
3. This notice may not be removed or altered from any source distribution.
