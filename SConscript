# Phobos library SConscript file
#
#   Defines targets and relations between them.
#
#   Correct environment, build commands, etc. are defined by SConstruct.
import os
from os.path import relpath
import fnmatch
Import('env', 'OS')

# ZLIB library C sources, not likely to change
zlibSrc = Split("""
    adler32.c compress.c crc32.c deflate.c gzclose.c gzlib.c gzread.c gzwrite.c
    infback.c inffast.c inflate.c inftrees.c trees.c uncompr.c zutil.c
""")
env.Append(CFLAGS=["-I", relpath("etc/c/zlib")])
zlibSrc = map(lambda x: relpath("etc/c/zlib/"+x), zlibSrc)
zobjs = [env.CObj(c) for c in zlibSrc]
zlib = env.CLib("zlib", zobjs) 

# Hackish glob to include 4 levels for now.
# May use more of Python to get fully recursive listing.
dsources = Glob("std/*.d") + Glob("etc/*.d") + Glob("std/*/*.d") + Glob("std/*/*/*.d") + \
    Glob("std/*/*/*/*.d")

# Table to filter out foreign OS-specific modules
osList = [ "windows", "linux", "freebsd", "osx"]
osList.remove(OS) # remove our OS from the list

def ourPlatfrom(f):
    for os in osList:
        if f.path.find(os) != -1:
            return False
    return True

druntime = [env.subst("$DRUNTIME")] 
dsources = filter(ourPlatfrom, dsources)
env.DLib("phobos", dsources + zlib + druntime)
