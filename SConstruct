import sys
import re
import excons
import excons.cmake as cmake


env = excons.MakeBaseEnv()

out_libdir = excons.OutputBaseDirectory() + "/lib"

prjs = [
   {  "name": "zlib",
      "type": "cmake",
      "cmake-opts": {"AMD64": excons.GetArgument("AMD64", 0, int)},
      "cmake-cfgs": excons.CollectFiles(".", patterns=["CMakeLists.txt", "*.cmakein"], recursive=True),
      "cmake-srcs": excons.CollectFiles(".", patterns=["*.c", "*.S"], recursive=True)
   }
]

excons.AddHelpOptions(zlib="""CMAKE ZLIB OPTIONS
  AMD64=0|1 : Enable building amd64 assembly implementation""")
excons.DeclareTargets(env, prjs)



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
   env.Append(LIBPATH=[out_libdir])
   excons.Link(env, ZlibName(static=static), static=static, force=True, silent=True)

Export("ZlibName ZlibPath RequireZlib")
