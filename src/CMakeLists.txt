set(fmt_FOUND FALSE)
find_package(fmt QUIET CONFIG)

if(fmt_FOUND)
    message(STATUS "fmt found")
else()
    message(STATUS "fmt not found - fallback")

    include(FetchContent)

    FetchContent_Declare(
        fmt
        GIT_REPOSITORY https://github.com/fmtlib/fmt.git
        GIT_TAG main
    )

    FetchContent_MakeAvailable(fmt)
endif()

include(FetchContent)
# ----------------------------------------
# Fetch GLFW
# ----------------------------------------
FetchContent_Declare(
  glfw
  GIT_REPOSITORY https://github.com/glfw/glfw.git
  GIT_TAG        3.3.8
)
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(glfw)

# ----------------------------------------
# Fetch ImGui
# ----------------------------------------
FetchContent_Declare(
  imgui
  GIT_REPOSITORY https://github.com/ocornut/imgui.git
  GIT_TAG        v1.90.4
)
FetchContent_MakeAvailable(imgui)

# ImGui Backends manuell hinzufügen (GLFW + OpenGL3)
set(IMGUI_BACKEND_DIR ${imgui_SOURCE_DIR}/backends)
set(IMGUI_SRC
    ${imgui_SOURCE_DIR}/imgui.cpp
    ${imgui_SOURCE_DIR}/imgui_demo.cpp
    ${imgui_SOURCE_DIR}/imgui_draw.cpp
    ${imgui_SOURCE_DIR}/imgui_tables.cpp
    ${imgui_SOURCE_DIR}/imgui_widgets.cpp
    ${IMGUI_BACKEND_DIR}/imgui_impl_glfw.cpp
    ${IMGUI_BACKEND_DIR}/imgui_impl_opengl3.cpp
)

# ----------------------------------------
# OpenGL
# ----------------------------------------
find_package(OpenGL REQUIRED)

add_executable(${CMAKE_PROJECT_NAME} "main.cpp" ${IMGUI_SRC})

target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
    ${imgui_SOURCE_DIR}
    ${IMGUI_BACKEND_DIR}
)

target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE fmt::fmt glfw OpenGL::GL)