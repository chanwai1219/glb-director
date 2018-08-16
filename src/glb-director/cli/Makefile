# BSD 3-Clause License
# 
# Copyright (c) 2018 GitHub.
# All rights reserved.
# 
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
# 
# * Redistributions of source code must retain the above copyright notice, this
#   list of conditions and the following disclaimer.
# 
# * Redistributions in binary form must reproduce the above copyright notice,
#   this list of conditions and the following disclaimer in the documentation
#   and/or other materials provided with the distribution.
# 
# * Neither the name of the copyright holder nor the names of its
#   contributors may be used to endorse or promote products derived from
#   this software without specific prior written permission.
# 
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
# FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
# DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
# SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
# CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
# OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

all: glb-director-cli glb-config-check glb-director-pcap glb-director-stub-server

CHECK_SRCS = config_check.c \
	../glb_control_loop.c \
	../glb_fwd_config.c \
	../bind_classifier.c \
	../glb_director_config.c \
	../glb_encap.c \
	../siphash24.c \
	../shared_opt.c

PCAP_SRCS = pcap_mode.c \
	../glb_fwd_config.c \
	../glb_director_config.c \
	../glb_encap.c \
	../cmdline_parse.c \
	../cmdline_parse_etheraddr.c \
	../glb_encap_pcap.c \
	../siphash24.c \
	../shared_opt.c

STUB_SRCS = stub_server.c \
	../glb_fwd_config.c \
	../glb_director_config.c \
	../glb_encap.c \
	../cmdline_parse.c \
	../cmdline_parse_etheraddr.c \
	../glb_encap_pcap.c \
	../siphash24.c \
	../shared_opt.c

CHECK_TMP := $(patsubst ../%.c,./%.o,$(CHECK_SRCS))
CHECK_OBJ := $(patsubst %.c,%.o,$(CHECK_TMP))

PCAP_TMP := $(patsubst ../%.c,./%.o,$(PCAP_SRCS))
PCAP_OBJ := $(patsubst %.c,%.o,$(PCAP_TMP))

STUB_TMP := $(patsubst ../%.c,./%.o,$(STUB_SRCS))
STUB_OBJ := $(patsubst %.c,%.o,$(STUB_TMP))

CFLAGS += -Wall

WERROR_FLAGS += -Werror

CFLAGS += -O3 -g
CFLAGS += $(WERROR_FLAGS)
CFLAGS += -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=2
CFLAGS += -fstack-protector-all
CFLAGS += -DCLI_MODE
CFLAGS += -m64 -mssse3

LDFLAGS += -L/home/ubuntu/dpdk/x86_64-native-linuxapp-gcc/lib/
#LDFLAGS += -z relro -z now
LDFLAGS += -ljansson -lnuma -ldl -lpthread

glb-director-cli: main.c
	gcc -c $(CFLAGS) -I`pwd`/.. main.c
	gcc -c $(CFLAGS) -I`pwd`/.. ../siphash24.c
	gcc -o glb-director-cli main.o siphash24.o $(LDFLAGS)

glb-config-check:
	gcc -c $(CFLAGS) -I`pwd`/.. -I/home/ubuntu/dpdk/x86_64-native-linuxapp-gcc/include $(CHECK_SRCS)
	gcc -o glb-config-check $(CHECK_OBJ) $(LDFLAGS) -ldpdk -ldl -lnuma

glb-director-pcap:
	gcc -c $(CFLAGS) -I`pwd`/.. -I/home/ubuntu/dpdk/x86_64-native-linuxapp-gcc/include -DPCAP_MODE $(PCAP_SRCS)
	gcc -o glb-director-pcap $(PCAP_OBJ) $(LDFLAGS) -lpcap

glb-director-stub-server:
	gcc -c $(CFLAGS) -I`pwd`/.. -I/home/ubuntu/dpdk/x86_64-native-linuxapp-gcc/include -DPCAP_MODE $(STUB_SRCS)
	gcc -o glb-director-stub-server $(STUB_OBJ) $(LDFLAGS) -lpcap

clean:
	rm -rf glb-director-cli glb-config-check glb-director-pcap glb-director-stub-server
