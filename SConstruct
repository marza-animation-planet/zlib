import sys
import re
import excons


env = excons.MakeBaseEnv()


env.CMakeConfigure("zlib", opts={"AMD64": excons.GetArgument("AMD64", 0, int)})

out_incdir = excons.OutputBaseDirectory() + "/include"
out_libdir = excons.OutputBaseDirectory() + "/lib"

zconf_in = ["zconf.h.in"]
zconf_out = env.CMakeGenerated(out_incdir + "/zconf.h", zconf_in)

cmake_in = env.CMakeInputs(dirs=["."], patterns=[re.compile(r"^.*\.(cmakein|h|c|S)$")], exclude=zconf_in)
cmake_out = env.CMakeOutputs(exclude=zconf_out)

target = env.CMake(cmake_out, cmake_in)

env.CMakeClean()
env.Alias("zlib", target)

excons.SyncCache()


def RequireZlib(env, static=False):
   if not static:
      env.Append(CPPDEFINES=["ZLIB_DLL"])
   env.Append(CPPPATH=[out_incdir])
   env.Append(LIBPATH=[out_libdir])
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

