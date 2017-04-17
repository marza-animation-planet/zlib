import sys
import re
import excons
import excons.cmake as cmake


env = excons.MakeBaseEnv()

out_libdir = excons.OutputBaseDirectory() + "/lib"

def ZlibName(static=False):
   if sys.platform == "win32":
      libname = "zlib"
      if static:
         libname += "static"
   else:
      libname = "z"
   return libname

def ZlibPath(static=False):
   name = ZlibName(static=static)
   if sys.platform == "win32":
      libname = name + ".lib"
   else:
      libname = "lib" + name + (".a" if static else excons.SharedLibraryLinkExt())
   return out_libdir + "/" + libname

def RequireZlib(env, static=False):
   if not static:
      env.Append(CPPDEFINES=["ZLIB_DLL"])
   env.Append(CPPPATH=[excons.OutputBaseDirectory() + "/include"])
   excons.Link(env, ZlibPath(static=static), static=static, force=True, silent=True)


prjs = [
   {  "name": "zlib",
      "type": "cmake",
      "cmake-opts": {"AMD64": excons.GetArgument("zlib-amd64", 0, int)},
      "cmake-cfgs": excons.CollectFiles(".", patterns=["CMakeLists.txt", "*.cmakein"], recursive=True),
      "cmake-srcs": excons.CollectFiles(".", patterns=["*.c", "*.S"], recursive=True),
      "cmake-outputs": ["include/zlib.h",
                        "include/zconf.h",
                        ZlibPath(False),
                        ZlibPath(True)]
   }
]

excons.AddHelpOptions(zlib="""ZLIB OPTIONS
  zlib-amd64=0|1 : Enable building amd64 assembly implementation""")

excons.DeclareTargets(env, prjs)

Export("ZlibName ZlibPath RequireZlib")
