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
      "cmake-srcs": excons.CollectFiles(".", patterns=["*.c", "*.S", "*.cmakein", "CMakeLists.txt"], recursive=True)
   }
]

excons.DeclareTargets(env, prjs)


def RequireZlib(env, static=False):
   env.Append(CPPPATH=[excons.OutputBaseDirectory() + "/include"])
   env.Append(LIBPATH=[out_libdir])
   if not static:
      env.Append(CPPDEFINES=["ZLIB_DLL"])
      if sys.platform == "win32":
         env.Append(LIBS=["zlib"])
      else:
         if not excons.StaticallyLink(env, "z", silent=True):
            env.Append(LIBS=["z"])
   else:
      env.Append(LIBS=["zlibstatic" if sys.platform == "win32" else "z"])

def ZlibName(static=False):
   if sys.platform == "win32":
      basename = ("zlibstatic.lib" if static else "zlib.lib")
   else:
      basename = ("libz.a" if static else "libz.so")
   return out_libdir + "/" + basename

Export("RequireZlib ZlibName")

