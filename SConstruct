#=================================================
# The SCONSTRUCT file for building HIPO project.
# 
#=================================================
import glob
import os
import sys
import subprocess
#=================================================
# LOADING THE ENVIRONMENT
#=================================================
LOCAL_HIPO_INSTALL="/home/fbossu/software/hipo-io"

env = Environment(CPPPATH=["include",".","/usr/include","/usr/local/include","/opt/local/include","/group/clas12/packages/lz4/lib","/group/clas12/packages/hipo-io/libcpp",LOCAL_HIPO_INSTALL+"/libcpp"])
env.Append(ENV = os.environ)
env.Append(CPPPATH=["src/root","src/evio"])
env.Append(CCFLAGS=["-O2","-fPIC","-m64","-fmessage-length=0","-g","-std=c++11"])
env.Append(LIBPATH=["/opt/local/lib","/usr/lib","/usr/local/lib","/group/clas12/packages/lz4/lib","lib","/group/clas12/packages/hipo-io/lib",LOCAL_HIPO_INSTALL+"/lib"])
env.Append(CONFIGUREDIR=["/group/clas12/packages/lz4/lib","/group/clas12/packages/hipo-io/lib",LOCAL_HIPO_INSTALL+"/lib"])

# ROOT
try:
  root=root=os.environ["ROOTSYS"]
except:
  print "No ROOTSYS variable found, please check your installation"
  exit()

env.Append(CPPPATH=[subprocess.check_output([root+"/bin/root-config","--incdir"]).strip()])
env.Append(LIBPATH=[subprocess.check_output([root+"/bin/root-config","--libdir"]).strip()])
env.Append(CCFLAGS=[subprocess.check_output([root+"/bin/root-config","--libs"]).strip()])
rtmplibs=subprocess.check_output([root+"/bin/root-config","--libs"]).strip().split()
rootlibs = [ "lib"+l[2:] for l in rtmplibs ]
print rootlibs

#=================================================
# Check for compression libraries.
#=================================================
conf = Configure(env)

if conf.CheckLib('libhipo'):
   print '\n\033[32m[**] >>>>> found library : HIPO'
   print ''
   env.Append(CCFLAGS="-D__HIPO__")
    
if conf.CheckLib('liblz4'):
   print '\n\033[32m[**] >>>>> found library : LZ4'
   print '[**] >>>>> enabling lz4 compression. \033[0m'
   print ''
   env.Append(CCFLAGS="-D__LZ4__")

if conf.CheckLib('libz'):
   print '\n\033[32m[**] >>>>> found library : libz'
   print '[**] >>>>> enabling gzip compression. \033[0m'
   print ''
   env.Append(CCFLAGS="-D__LIBZ__")

for l in rootlibs:
  if conf.CheckLib(l):
    print l
    #env.Append(CCFLAGS="-D__"+l.upper()+"__")
  

if conf.CheckLib("libEG"):
  print "libEG"


#=================================================
# BUILDING EXECUTABLE PROGRAM
#=================================================
#runFileLoop   = env.Library(target="src/libParticle",source=["src/Particle.cpp"])
runFileLoop   = env.SharedLibrary(target="libAnalysis",source=[
                      "src/Analysis.cpp", 
                      "src/Particle.cpp",
                      "src/Event.cpp",
                      "src/Track.cpp"
])

#anSrcPath="/home/fbossu/software/Analysis/src"

#env.Append(LIBPATH=[anSrcPath,"."])
#env.Append(CPPPATH=[anSrcPath])
##conf.CheckLib("Particle")
##conf.CheckLib("Analysis")'
#runFileLoop   = env.Program(target="runFileLoop",
                            #source=[
                      #"runFileLoop.cpp",
                      #"AnDVCS.cpp"
                      ##"src/Particle.cpp",
                      ##"src/Analysis.cpp", 
                      ##"src/Event.cpp",
                      ##"src/Track.cpp"
                      #] )