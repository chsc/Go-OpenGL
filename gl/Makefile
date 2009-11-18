# Copyright 2009 The Go Authors.  All rights reserved.
# Use of this source code is governed by a BSD-style
# license that can be found in the LICENSE file.

include $(GOROOT)/src/Make.$(GOARCH)

TARG=gl

GOFILES:=gl_defs.go

CGOFILES:=gl.go

CGO_LDFLAGS:=-lGLEW

CGO_CFLAGS:=

CLEANFILES+=gl

include $(GOROOT)/src/Make.pkg

DOLLAR:="$"

gl_defs.c:
	echo -e '#define GL_GLEXT_PROTOTYPES\n#include <GL/gl.h>\n#include <GL/glext.h>\nenum\n{\n' > gl_defs.c
	cat /usr/include/GL/gl.h /usr/include/GL/glext.h | sed -rne 's/#define\s*GL_(\w*).*/\t~GL_\1 = GL_\1,/p' | sort | uniq | tr '~' '\044' >> gl_defs.c
	cat /usr/include/GL/gl.h /usr/include/GL/glext.h | sed -rne 's/#define\s*GL_([A-Z]\w*).*/\t~\1 = GL_\1,/p' | sort | uniq | tr '~' '\044' >> gl_defs.c
	echo '};' >> gl_defs.c

gl.go: gl.c convert.pl
	echo "Warning: regenerating gl.go"
	./convert.pl gl.c > gl.go

gl_defs.go: gl_defs.c
	godefs -g gl gl_defs.c > gl_defs.go
