# Copyright 2009 The Go Authors.  All rights reserved.
# Use of this source code is governed by a BSD-style
# license that can be found in the LICENSE file.

include $(GOROOT)/src/Make.inc

TARG=gl

GOFILES:=gl_defs.go

CGOFILES:=gl.go

ifeq ($(GOOS),darwin)
CGOFILES+=gl_darwin.go
CGO_LDFLAGS:=-framework OpenGL
else
CGOFILES+=gl_notdarwin.go
CGO_LDFLAGS:=-lGL
endif

CLEANFILES+=gl

include $(GOROOT)/src/Make.pkg

DOLLAR:="$"

gl_defs.c:
	echo '#define GL_GLEXT_PROTOTYPES\n#ifdef __APPLE__\n# include <OpenGL/gl.h>\n# include <OpenGL/glext.h>\n#else\n# include <GL/gl.h>\n# include <GL/glext.h>\n#endif\nenum\n{\n' > gl_defs.c
	cat /usr/include/GL/gl.h /usr/include/GL/glext.h | sed -rne 's/#define\s*GL_([0-9]\w*).*/\t~GL_\1 = GL_\1,/p' | egrep -vi "(_EXT|_NV|_ATI|_APPLE|_AMD|_MESA|_SUN|_GREMEDY|_SGI|_3DFX|_ARB|_INTEL|_IBM|_OML|_HP|_WIN|_OES|_INGR|_PGI|_S3)" | sort | uniq | tr '~' '\044' >> gl_defs.c
	cat /usr/include/GL/gl.h /usr/include/GL/glext.h | sed -rne 's/#define\s*GL_([A-Z]\w*).*/\t~\1 = GL_\1,/p' | egrep -vi "(_EXT|_NV|_ATI|_APPLE|_AMD|_MESA|_SUN|_GREMEDY|_SGI|_3DFX|_ARB|_INTEL|_IBM|_OML|_HP|_WIN|_OES|_INGR|_PGI|_S3)" | sort | uniq | tr '~' '\044' >> gl_defs.c
	echo '};' >> gl_defs.c

gl_defs.go: gl_defs.c
	godefs `echo $(CGO_CFLAGS) | sed 's/\([^ ^$(DOLLAR)]*\)/-f \1/g'` -g gl gl_defs.c > gl_defs.go
	gofmt -w gl_defs.go
