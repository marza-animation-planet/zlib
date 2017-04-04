import sys
import re
import excons
import excons.cmake as cmake


env = excons.MakeBaseEnv()

prjs = [
   {  "name": "zlib",
      "type": "cmake",
      "cmake-opts": {"AMD64": excons.GetArgument("AMD64", 0, int)},
      "cmake-srcs": excons.CollectFiles(".", patterns=["*.c", "*.S", "*.cmakein"], recursive=True)
   }
]

excons.DeclareTargets(env, prjs)


def RequireZlib(env, static=False):
   if not static:
      env.Append(CPPDEFINES=["ZLIB_DLL"])
   env.Append(CPPPATH=[excons.OutputBaseDirectory() + "/include"])
   env.Append(LIBPATH=[excons.OutputBaseDirectory() + "/lib"])
   if sys.platform != "win32":
      if static:
         excons.StaticallyLink(env, "z", silent=True)
      else:
         env.Append(LIBS=["z"])
   else:
      if static:
         env.Append(LIBS=["zlibstatic"])
      else:
         env.Append(LIBS=["zlib"])

Export("RequireZlib")

