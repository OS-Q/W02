MCU     ?= stm8s003f3
FAMILY  ?= STM8S
ARCH  	= stm8
LIBDIR	= ../include
F_CPU   ?= 2000000
TARGET  ?= main.ihx
NAME  	?= app
SRCS    := $(wildcard *.c)
OBJS     = $(SRCS:.c=.rel)

CC       = sdcc
OBJCOPY  = sdobjcopy
PACK_HEX = packihx
CFLAGS   = -m$(ARCH) -p$(MCU) -D$(FAMILY)
CFLAGS  += -DF_CPU=$(F_CPU)UL -I. -I$(LIBDIR)
CFLAGS  += --stack-auto --noinduction --use-non-free
LDFLAGS  = -m$(ARCH) -l$(ARCH) --out-fmt-ihx
## RELOCATE_IVT set to 0:
#LDFLAGS += --code-loc 0x8200
## RELOCATE_IVT set to 1:
LDFLAGS += --code-loc 0x8280

all: $(TARGET) size

$(TARGET): $(OBJS)
	$(CC) $(LDFLAGS) $(OBJS) -o $@
	$(PACK_HEX) $(TARGET) > $(NAME).hex
	@$(OBJCOPY) -I ihex --output-target=binary $(TARGET) $(NAME).bin
	cp $(NAME).bin ../build/
	cp $(NAME).hex ../build/

size:
	@echo "-----\nApp Image size:"
	@stat -L -c %s $(NAME).bin

%.rel: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f *.map *.asm *.rel *.ihx *.o *.sym *.lk *.lst *.rst *.cdb *.bin

.PHONY: clean all
