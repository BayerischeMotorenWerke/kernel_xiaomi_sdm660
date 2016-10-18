#
# linux/arch/arm/tools/Makefile
#
# Copyright (C) 2001 Russell King
#

gen := arch/$(ARCH)/include/generated
kapi := $(gen)/asm

kapi-hdrs-y := $(kapi)/mach-types.h

targets += $(addprefix ../../../,$(kapi-hdrs-y))

PHONY += kapi

kapi:	$(kapi-hdrs-y)

# Create output directory if not already present
_dummy := $(shell [ -d '$(kapi)' ] || mkdir -p '$(kapi)')

$(kapi)/mach-types.h: $(src)/gen-mach-types $(src)/mach-types
	@$(kecho) '  Generating $@'
	@mkdir -p $(dir $@)
	$(Q)$(AWK) -f $^ > $@ || { rm -f $@; /bin/false; }

quiet_cmd_syshdr = SYSHDR  $@
      cmd_syshdr = $(CONFIG_SHELL) '$(syshdr)' '$<' '$@' \
		   '$(syshdr_abi_$(basetarget))' \
		   '$(syshdr_pfx_$(basetarget))' \
		   '__NR_SYSCALL_BASE'

quiet_cmd_systbl = SYSTBL  $@
      cmd_systbl = $(CONFIG_SHELL) '$(systbl)' '$<' '$@' \
		   '$(systbl_abi_$(basetarget))'

quiet_cmd_sysnr  = SYSNR   $@
      cmd_sysnr  = $(CONFIG_SHELL) '$(sysnr)' '$<' '$@' \
		   '$(syshdr_abi_$(basetarget))'

syshdr_abi_unistd-common := common
$(uapi)/unistd-common.h: $(syscall) $(syshdr) FORCE
	$(call if_changed,syshdr)

syshdr_abi_unistd-oabi := oabi
$(uapi)/unistd-oabi.h: $(syscall) $(syshdr) FORCE
	$(call if_changed,syshdr)

syshdr_abi_unistd-eabi := eabi
$(uapi)/unistd-eabi.h: $(syscall) $(syshdr) FORCE
	$(call if_changed,syshdr)

sysnr_abi_unistd-nr := common,oabi,eabi,compat
$(kapi)/unistd-nr.h: $(syscall) $(sysnr) FORCE
	$(call if_changed,sysnr)

systbl_abi_calls-oabi := common,oabi
$(gen)/calls-oabi.S: $(syscall) $(systbl) FORCE
	$(call if_changed,systbl)

systbl_abi_calls-eabi := common,eabi
$(gen)/calls-eabi.S: $(syscall) $(systbl) FORCE
	$(call if_changed,systbl)
