---
description: 简单的 ImGui 上手教程
---

# ImGui

首先需要将 ImGui 工程拷贝到本地，项目的 [Github 地址](https://github.com/ocornut/imgui)

这里使用的是 [premake5](https://premake.github.io/) 的构建工具，需要下载下来，可以直接使用已经编译好的 exe 文件。这个构建是基于 lua 脚本来生成工程的，所以我们需要编写相应的 lua 脚本来生成工程。

把 ImGui 下载下来之后，在项目的根目录下，新建一个 premake5.lua 脚本，内容如下：

```lua
project "ImGui"
	kind "StaticLib"
	language "C++"
        staticruntime "off"

	targetdir ("bin/" .. outputdir .. "/%{prj.name}")
	objdir ("bin-int/" .. outputdir .. "/%{prj.name}")

	files
	{
		"imconfig.h",
		"imgui.h",
		"imgui.cpp",
		"imgui_draw.cpp",
		"imgui_internal.h",
		"imgui_tables.cpp",
		"imgui_widgets.cpp",
		"imstb_rectpack.h",
		"imstb_textedit.h",
		"imstb_truetype.h",
		"imgui_demo.cpp"
	}

	filter "system:windows"
		systemversion "latest"
		cppdialect "C++17"

	filter "system:linux"
		pic "On"
		systemversion "latest"
		cppdialect "C++17"

	filter "configurations:Debug"
		runtime "Debug"
		symbols "on"

	filter "configurations:Release"
		runtime "Release"
		optimize "on"

        filter "configurations:Dist"
		runtime "Release"
		optimize "on"
                symbols "off"
```

如果说要将其加入到 git 项目中，需要 fork 这个仓库，然后添加 premake5.lua 这个文件，然后提交，之后再去为你的 git 项目添加 submodule 。

创建主项目的 premake.lua 文件，内容如下：

```lua
workspace "gui"
	architecture "x64"

	configurations 
	{
		"Debug",
		"Release",
	}

outputdir = "%{cfg.buildcfg}-%{cfg.system}-%{cfg.architecture}"

IncludeDir = {}
IncludeDir["imgui"] = "gui/vendor/imgui"

include "gui/vendor/imgui"

project "gui"
	location "gui"
	kind "ConsoleApp"
	language "C++"

	targetdir ("bin/" .. outputdir .. "/%{prj.name}")
	objdir ("bin-int/" .. outputdir .. "/%{prj.name}")

	files
	{
		"%{prj.name}/src/**.h",
		"%{prj.name}/src/**.cpp"
	}

	includedirs
	{
		"%{prj.name}/src",
		"%{IncludeDir.imgui}"
	}

	links 
	{
		"ImGui",
	}

	filter "system:windows"
		cppdialect "C++17"
		staticruntime "On"
		systemversion "latest"

	filter "configurations:Debug"
		buildoptions "/MDd"
		symbols "On"
		

	filter "configurations:Release"
		buildoptions "/MD"
		optimize "On"
```

因为 ImGui 是需要依赖系统提供的 UI 接口的，然后去创建相应的窗口和交互信息。它支持的后台有安卓、DirectX、OpenGL、glfw、OSX、Vulkan、win32等等，所以还需要去把后台搭建好。

同样需要下载 [GLFW ](https://github.com/glfw/glfw)项目，下载完成后添加 premake5.lua，内容如下：

```lua
project "GLFW"
	kind "StaticLib"
	language "C"
	staticruntime "off"

	targetdir ("bin/" .. outputdir .. "/%{prj.name}")
	objdir ("bin-int/" .. outputdir .. "/%{prj.name}")

	files
	{
		"include/GLFW/glfw3.h",
		"include/GLFW/glfw3native.h",
		"src/glfw_config.h",
		"src/context.c",
		"src/init.c",
		"src/input.c",
		"src/monitor.c",

		"src/null_init.c",
		"src/null_joystick.c",
		"src/null_monitor.c",
		"src/null_window.c",

		"src/platform.c",
		"src/vulkan.c",
		"src/window.c",
	}

	filter "system:linux"
		pic "On"

		systemversion "latest"
		
		files
		{
			"src/x11_init.c",
			"src/x11_monitor.c",
			"src/x11_window.c",
			"src/xkb_unicode.c",
			"src/posix_time.c",
			"src/posix_thread.c",
			"src/glx_context.c",
			"src/egl_context.c",
			"src/osmesa_context.c",
			"src/linux_joystick.c"
		}

		defines
		{
			"_GLFW_X11"
		}

	filter "system:windows"
		systemversion "latest"

		files
		{
			"src/win32_init.c",
			"src/win32_joystick.c",
			"src/win32_module.c",
			"src/win32_monitor.c",
			"src/win32_time.c",
			"src/win32_thread.c",
			"src/win32_window.c",
			"src/wgl_context.c",
			"src/egl_context.c",
			"src/osmesa_context.c"
		}

		defines 
		{ 
			"_GLFW_WIN32",
			"_CRT_SECURE_NO_WARNINGS"
		}

		links
		{
			"Dwmapi.lib"
		}

	filter "configurations:Debug"
		runtime "Debug"
		symbols "on"

	filter "configurations:Release"
		runtime "Release"
		optimize "on"

	filter "configurations:Dist"
		runtime "Release"
		optimize "on"
        symbols "off"
```

项目自身的 premake5.lua 如下：

```lua
workspace "gui"
	architecture "x64"

	configurations 
	{
		"Debug",
		"Release",
	}

outputdir = "%{cfg.buildcfg}-%{cfg.system}-%{cfg.architecture}"

IncludeDir = {}
IncludeDir["imgui"] = "gui/vendor/imgui"
IncludeDir["GLFW"] = "gui/vendor/GLFW/include"

include "gui/vendor/GLFW"
include "gui/vendor/imgui"

project "gui"
	location "gui"
	kind "ConsoleApp"
	language "C++"

	targetdir ("bin/" .. outputdir .. "/%{prj.name}")
	objdir ("bin-int/" .. outputdir .. "/%{prj.name}")

	files
	{
		"%{prj.name}/src/**.h",
		"%{prj.name}/src/**.cpp"
	}

	includedirs
	{
		"%{prj.name}/src",
		"%{IncludeDir.imgui}",
		"%{IncludeDir.GLFW}"
	}

	links 
	{
		"GLFW",
		"ImGui",
		"opengl32.lib"
	}

	filter "system:windows"
		cppdialect "C++17"
		staticruntime "On"
		systemversion "latest"

	filter "configurations:Debug"
		buildoptions "/MDd"
		symbols "On"
		

	filter "configurations:Release"
		buildoptions "/MD"
		optimize "On"
	
	
```



