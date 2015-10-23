CFLAGS  := --std=gnu99 -g -Os -I. $(EXTRACFLAGS)
CFLAGS  += -Wall -Wmissing-declarations
CFLAGS  += -ffunction-sections -fdata-sections
#CFLAGS += -DDEBUG_PACKET -DDEBUG -g3
LDFLAGS += -Wl,--gc-sections -Llibubox -Wl,-Bstatic -lubox -Wl,-Bdynamic

TARGET = rmbim
SOURCES = cli.c  mbim-dev.c  mbim-enum.c  mbim-msg.c

OBJS=$(SOURCES:.c=.o)


PREFIX=/usr

default_target: all
.PHONY : default_target

install: all
	install -D $(TARGET) $(INSTALLDIR)/$(PREFIX)/sbin/$(TARGET)
	$(STRIP) $(INSTALLDIR)/$(PREFIX)/sbin/$(TARGET)
.PHONY : install

clean:
	make -C libubox clean
	@rm -f $(OBJS) $(TARGET) $(service_sources) $(service_headers) qmi-errors.c
.PHONY : clean

libubox:
	make -C libubox
.PHONY : libubox

# The main all target
all: $(TARGET)
.PHONY : all

$(TARGET): libubox $(OBJS)
	$(CC) -o $@ $(OBJS) $(LDFLAGS)

.c.o::
	$(CC) $(CFLAGS) -c $<

# Dependencies
cli.o: cli.c mbim.h
mbim-dev.o: mbim-dev.c mbim.h
mbim-msg.o: mbim-msg.c mbim.h

# $(service_headers)
#commands.o: commands.c rmbim.h commands.h $(service_sources) $(service_headers)
#qmi-message.o: qmi-message.c qmi-message.h $(service_headers)
