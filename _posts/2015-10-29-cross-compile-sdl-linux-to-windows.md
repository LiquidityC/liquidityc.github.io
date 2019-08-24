---
title:  "Cross-compiling SDL from linux to windows (mingw/make)"
layout: post
date:   2015-10-29 09:58
categories: compile sdl crosscompile windows
comments: true
---

So, I struggled __a lot__ with this lately but finally got a standalone
executable that runs on windows with all the libraries included with
my game.

First of all comes the install of the mingw compiler libs. I'm on an
archlinux system. If you aren't you will have to adapt to your own distro
but I don't think that should be too hard. I might have forgotten some
of the libs that I actually needed but it's not really relevant to the
post. You'll have to adapt to your own project.

### Installing libs
The following is what I installed:

    - mingw-w64 (mingw compiler)
    - mingw-w64-sdl2
    - mingw-w64-sdl2_image
    - mingw-w64-sdl2_mixer
    - mingw-w64-sdl2_ttf
    - mingw-w64-sqlite
    - wine (not needed but REALLY nice for testing)

I could get all of these in one nice command (Archlinux): 

{% highlight bash %}
yaourt -S wine mingw-w64 mingw-w64-sdl2 mingw-w64-sdl2_image mingw-w64-sdl2_mixer mingw-w64-sdl2_ttf mingw-w64-sqlite
{% endhighlight %}

### The Makefile
Below will follow snippets of the Makefile that I produced. I named it
'Makefile.win' and it lives right next to my regular Makefile.

First set the variables and compiler flag. I set the compiler 'CXX'
to mingw using absolute path. I also produce the correct flags based
on the libraries that I just installed. I hope this isn't too hard to
follow if you have some knowledge of Make.

{% highlight make %}
CXX = /usr/bin/x86_64-w64-mingw32-g++

SDL_ROOT_DIR    = /usr/x86_64-w64-mingw32/bin

SDL2_CFLAGS     = `$(SDL_ROOT_DIR)/sdl2-config --cflags`

SDL2_LDFLAGS    = `$(SDL_ROOT_DIR)/sdl2-config --libs` \
                  -lSDL2_image.dll -lSDL2_ttf.dll -lSDL2_mixer.dll \
                  -static-libgcc -static-libstdc++

CXXFLAGS        = -c -g -pedantic -Wall -Wpointer-arith -Wcast-qual -std=c++11 \
                      -Iflat/include -Iinclude $(SDL2_CFLAGS) 

LD              = /usr/bin/x86_64-w64-mingw32-g++
LDFLAGS         = -L./flat/lib/ -lflat.dll $(SDL2_LDFLAGS)
{% endhighlight %}

After that we tell make how to create object files from cpp files. Note that I named my mingw objectfiles .o32 in order to separate them from my regular build process. I highly recommend this.

{% highlight make %}
OBJDIR              = obj
EXECUTABLE          = justdontdie.exe
PROG_SOURCES        = $(wildcard src/*.cpp)
PROG_OBJECTS        = $(addprefix $(OBJDIR)/,$(notdir $(PROG_SOURCES:.cpp=.o32)))

$(OBJDIR)/%.o32: src/%.cpp
    $(CXX) $(CXXFLAGS) $< -o $@

$(EXECUTABLE): $(PROG_OBJECTS)
    $(LD) $(PROG_OBJECTS) -o $@ $(LDFLAGS)
{% endhighlight %}

That is more or less it for the actual build process. Once you get your
executable to compile it's time to test it. Copy all the dll's that your
program needs into the same folder as your executable (I added this to my
Makefile). On my system they were located in '/usr/x86_64-w64-mingw32/bin'
and they shared the name 'almost' with the libraries you included
during compile.

### Testing
Once you have your executable and all the needed dlls in the same
folder it's time to test. This is where wine will become very useful. I
initally tested my .exe on windows. Windows then told me what main dlls
that were missing if any, however, windows did not tell me what dlls
needed by included dlls that were missing. It would just fail to start
the program and I had no clue what was going wrong.  When I got around
to testing my .exe in wine, wine would actually tell me exactly what
dlls were missing and what they were needed for. This helped _ALOT_.
Once you get your program to run on wine it *should* be pretty safe to
assume that it will run on actual windows as well.

I hope this helps people because getting this to work was a big hurdle
in my development process and I personally thought that there was little
information to find via google.

I'll include my actual Makefile.win below. Note that this also builds a
library that I built and copys around a bunch of files so it might not
be as easy to read. I still want to include it though, for completeness
and in case I missed something in the sections above.

Good luck with your compiling.

// Liq

PS. [this](https://liquidityc.github.io/justdontdie) is the game I'm compiling for windows.

### My complete Makefile.win
{% highlight make %}

CXX = /usr/bin/x86_64-w64-mingw32-g++

# ECHO colors
CNORMAL = \033[0m
CGREEN  = \033[32m

MFLAGS = -f Makefile.win

SDL_ROOT_DIR    = /usr/x86_64-w64-mingw32/bin

SDL2_CFLAGS     = `$(SDL_ROOT_DIR)/sdl2-config --cflags`

SDL2_LDFLAGS    = `$(SDL_ROOT_DIR)/sdl2-config --libs` \
                  -lSDL2_image.dll -lSDL2_ttf.dll -lSDL2_mixer.dll \
                  -static-libgcc -static-libstdc++

CXXFLAGS        = -c -g -pedantic -Wall -Wpointer-arith -Wcast-qual -std=c++11 \
                  -Iflat/include -Iinclude $(SDL2_CFLAGS) 

DISTDIR     = dist/win
DLLSRCDIR   = /usr/x86_64-w64-mingw32/bin
DLLS        = SDL2.dll SDL2_image.dll SDL2_mixer.dll SDL2_ttf.dll libbz2-1.dll libfreetype-6.dll libpng16-16.dll \
              libwinpthread-1.dll zlib1.dll libvorbisfile-3.dll libvorbis-0.dll libogg-0.dll

LD          = /usr/bin/x86_64-w64-mingw32-g++
LDFLAGS     = -L./flat/lib/ -lflat.dll $(SDL2_LDFLAGS)
RM          = rm
ECHO        = echo
CP          = cp
MV          = mv

OBJDIR              = obj
DEPS                = $(wildcard src/*.h) flat/lib/libflat.dll.a
EXECUTABLE          = $(DISTDIR)/justdontdie.exe
PROG_SOURCES        = $(wildcard src/*.cpp)
PROG_OBJECTS        = $(addprefix $(OBJDIR)/,$(notdir $(PROG_SOURCES:.cpp=.o32)))

LIBRARIES           = flat

.PHONY: $(LIBRARIES) $(OBJDIR) libs default clean cleanall copydlls copyresources buildzip

default: 
    @$(ECHO) -e "$(CGREEN)Creating distribution directory $(DISTDIR)$(CNORMAL)"
    @mkdir -p $(DISTDIR)
    @$(MAKE) $(MFLAGS) --no-print-directory libs
    @$(ECHO) -e "$(CGREEN)Building $(EXECUTABLE)...$(CNORMAL)"
    @$(MAKE) $(MFLAGS) --no-print-directory '$(EXECUTABLE)'
    @$(ECHO) -e "$(CGREEN)Building $(EXECUTABLE) complete$(CNORMAL)"
    @$(ECHO) -e "$(CGREEN)Copying dlls$(CNORMAL)"
    @$(MAKE) $(MFLAGS) --no-print-directory copydlls
    @$(ECHO) -e "$(CGREEN)Copying resources$(CNORMAL)"
    @$(MAKE) $(MFLAGS) --no-print-directory copyresources
    @$(ECHO) -e "$(CGREEN)Creating zip file$(CNORMAL)"
    @$(MAKE) $(MFLAGS) --no-print-directory buildzip
    @$(ECHO) -e "$(CGREEN)Done$(CNORMAL)"

all: default

libs: $(LIBRARIES)

$(EXECUTABLE): $(PROG_OBJECTS) $(DEPS)
    $(LD) $(PROG_OBJECTS) -o $@ $(LDFLAGS)

$(OBJDIR)/%.o32: src/%.cpp
    $(CXX) $(CXXFLAGS) $< -o $@

$(LIBRARIES):
    @$(ECHO) -e "$(CGREEN)Building $@lib...$(CNORMAL)"
    @$(MAKE) -C $@ $(MFLAGS)
    @$(ECHO) -e "$(CGREEN)Building $@lib complete$(CNORMAL)"

copydlls:
    @for f in $(DLLS); do ($(CP) $(DLLSRCDIR)/$$f $(DISTDIR)); done

copyresources:
    @$(CP) -r resources $(DISTDIR)/.

buildzip:
    zip -r justdontdie.zip $(DISTDIR)

clean:
    @$(ECHO) Cleaning project
    @$(RM) -f $(EXECUTABLE) $(PROG_OBJECTS)

cleanall:
    @$(ECHO) Cleaning project and all libraries
    @for d in $(LIBRARIES); do (cd $$d; $(MAKE) $(MFLAGS) --no-print-directory clean ); done
    @$(RM) -f $(EXECUTABLE) $(PROG_OBJECTS)
{% endhighlight %}
