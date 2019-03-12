############################################################################
# arch/cortex-m23/m2351/src/numaker_pfm_m2351/nonsecure/Makefile
#
# Copyright (c) 2019 Samsung Electronics Co., Ltd. All Rights Reserved.
# Author: Taras Drozdovskyi t.drozdovsky@samsung.com
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
# 
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
############################################################################

-include $(TOPDIR)/Make.defs
-include $(TOPDIR)/.config
-include Make.defs

CFLAGS += -I../ -I./	
CFLAGS += -I../../CMSIS/Include
CFLAGS += -I../../StdDriver/inc
CFLAGS += -I../../Device/Nuvoton/M2351/Include

CFLAGS += -DDEBUG_PORT=UART1_NS

OBJDIR = $(TOPDIR)/build/nonsecure$(subst $(TOPDIR),,$(shell pwd))

MTOWER_NS = $(TOPDIR)/mtower_ns$(EXEEXT)

ASRCS_NS = $(CHIP_ASRCS_NS) $(CMN_ASRCS)
AOBJS_NS = $(addprefix $(OBJDIR)/, $(ASRCS_NS:.S=$(OBJEXT)))

CSRCS_NS = $(CHIP_CSRCS_NS) $(CMN_CSRCS)
COBJS_NS = $(addprefix $(OBJDIR)/, $(CSRCS_NS:.c=$(OBJEXT)))

OBJS_NS = $(AOBJS_NS) $(COBJS_NS)

LIBPATHS = -L. -L"$(TOPDIR)/lib"

LIBS =  -lm -lc -lnosys -lm2351_StdDriver_ns -lnsc

$(AOBJS_NS): $(OBJDIR)/%$(OBJEXT): %.S
	$(Q) mkdir -p $(OBJDIR)/$(dir $<)
	@echo "AS: $<"
	$(Q) $(CC) -c $(AFLAGS) $< -o $@

$(COBJS_NS): $(OBJDIR)/%$(OBJEXT): %.c
	$(Q) mkdir -p $(OBJDIR)/$(dir $<)
	@echo "CC: $<"
	$(Q) $(CC) -c $(CFLAGS) $< -o $@

mtower_ns$(EXEEXT): $(OBJS_NS) 
	$(Q) $(MAKE) -C ../../StdDriver/src/ TOPDIR="$(TOPDIR)" libm2351_StdDriver_ns.a
	@echo "LD: mTower_ns$(EXEEXT)"
	$(Q) $(CC) $(CFLAGS) -Tnonsecure.ld $(OBJS_NS) $(LIBPATHS) $(LIBS) -o $(MTOWER_NS)
	$(Q) $(OBJCOPY) -O ihex $(MTOWER_NS) $(TOPDIR)/mtower_ns.hex
	$(Q) $(OBJCOPY) -O binary $(MTOWER_NS) $(TOPDIR)/mtower_ns.bin

clean:
	$(Q) rm -f *$(OBJEXT) *$(LIBEXT)
	$(Q) rm -f ../../Device/Nuvoton/M2351/NonSource/*$(OBJEXT)
	$(Q) rm -f ../../Device/Nuvoton/M2351/NonSource/GCC/*$(OBJEXT)
	$(Q) rm -f ../../StdDriver/src/*$(OBJEXT) ../../StdDriver/src/*$(LIBEXT)

distclean: clean
