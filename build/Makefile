########################################
#  This file was generated by hdlmake  #
#  http://ohwr.org/projects/hdl-make/  #
########################################

TOP_MODULE := ebaz4205_top
PROJECT := ebaz4205_top
PROJECT_FILE := $(PROJECT).xpr
TOOL_PATH := /tools/Xilinx/Vivado/2018.2/bin
TCL_INTERPRETER := vivado -mode tcl -source
ifneq ($(strip $(TOOL_PATH)),)
TCL_INTERPRETER := $(TOOL_PATH)/$(TCL_INTERPRETER)
endif

SYN_FAMILY := 
SYN_DEVICE := xc7z010
SYN_PACKAGE := clg400
SYN_GRADE := -1

TCL_CREATE := create_project $(PROJECT) ./
TCL_OPEN := open_project $(PROJECT_FILE)
TCL_CLOSE := exit
ifneq ($(wildcard $(PROJECT_FILE)),)
TCL_CREATE := $(TCL_OPEN)
endif

#target for performing local synthesis
all: bitstream

SOURCES_VHDLFile := \
../src/hdl/ebaz4205_top.vhd \
../src/hdl/system_wrapper.vhd

SOURCES_XDCFile := \
../src/xilinx/xdc/bitstream.xdc \
../src/xilinx/xdc/general.xdc \
../src/xilinx/xdc/pinout.xdc

SOURCES_TCLFile := \
../src/xilinx/bd/system.tcl

files.tcl:
		@$(foreach sourcefile, $(SOURCES_VHDLFile), echo "add_files -norecurse $(sourcefile); set_property IS_GLOBAL_INCLUDE 1 [get_files $(sourcefile)]" >> $@ &)
		@$(foreach sourcefile, $(SOURCES_XDCFile), echo "add_files -norecurse $(sourcefile); set_property IS_GLOBAL_INCLUDE 1 [get_files $(sourcefile)]" >> $@ &)
		@$(foreach sourcefile, $(SOURCES_TCLFile), echo "source $(sourcefile)" >> $@ &)

SYN_PRE_PROJECT_CMD := 
SYN_POST_PROJECT_CMD := 

SYN_PRE_SYNTHESIZE_CMD := 
SYN_POST_SYNTHESIZE_CMD := 

SYN_PRE_PAR_CMD := 
SYN_POST_PAR_CMD := 

SYN_PRE_BITSTREAM_CMD := 
SYN_POST_BITSTREAM_CMD := $(TCL_INTERPRETER) syn_post_bitstream_cmd.tcl $(TCL_OPEN)

project.tcl:
		echo $(TCL_CREATE) >> $@
		echo # project properties >> $@
		echo set_property "part" "$(SYN_DEVICE)$(SYN_PACKAGE)$(SYN_GRADE)" [current_project] >> $@
		echo set_property "target_language" "vhdl" [current_project] >> $@
		echo set_property "top" "$(TOP_MODULE)" [get_property srcset [current_run]] >> $@
		echo source files.tcl >> $@
		echo update_compile_order -fileset sources_1 >> $@
		echo update_compile_order -fileset sim_1 >> $@
		echo $(TCL_CLOSE) >> $@

project: files.tcl project.tcl
		$(SYN_PRE_PROJECT_CMD)
		$(TCL_INTERPRETER) $@.tcl
		$(SYN_POST_PROJECT_CMD)
		touch $@

synthesize.tcl:
		echo $(TCL_OPEN) >> $@
		echo # synthesize properties >> $@
		echo set_property -name {steps.synth_design.args.more options} -value {-verbose} -objects [get_runs synth_1] >> $@
		echo set_property "steps.synth_design.args.assert" "1" [get_runs synth_1] >> $@
		echo reset_run synth_1 >> $@
		echo launch_runs synth_1 >> $@
		echo wait_on_run synth_1 >> $@
		echo set result [get_property STATUS [get_runs synth_1]] >> $@
		echo set keyword [lindex [split '$$'result " "] end] >> $@
		echo if { '$$'keyword != \"Complete!\" } { >> $@
		echo     exit 1 >> $@
		echo } >> $@
		echo $(TCL_CLOSE) >> $@

synthesize: project synthesize.tcl
		$(SYN_PRE_SYNTHESIZE_CMD)
		$(TCL_INTERPRETER) $@.tcl
		$(SYN_POST_SYNTHESIZE_CMD)
		touch $@

par.tcl:
		echo $(TCL_OPEN) >> $@
		echo # par properties >> $@
		echo set_property "steps.opt_design.args.verbose" "1" [get_runs impl_1] >> $@
		echo set_property "steps.opt_design.is_enabled" "1" [get_runs impl_1] >> $@
		echo set_property -name {steps.place_design.args.more options} -value {-verbose} -objects [get_runs impl_1] >> $@
		echo set_property -name {steps.phys_opt_design.args.more options} -value {-verbose} -objects [get_runs impl_1] >> $@
		echo set_property "steps.phys_opt_design.is_enabled" "1" [get_runs impl_1] >> $@
		echo set_property -name {steps.route_design.args.more options} -value {-verbose} -objects [get_runs impl_1] >> $@
		echo set_property -name {steps.post_route_phys_opt_design.args.more options} -value {-verbose} -objects [get_runs impl_1] >> $@
		echo set_property "steps.post_route_phys_opt_design.is_enabled" "1" [get_runs impl_1] >> $@
		echo set_property "steps.write_bitstream.args.bin_file" "1" [get_runs impl_1] >> $@
		echo reset_run impl_1 >> $@
		echo launch_runs impl_1 >> $@
		echo wait_on_run impl_1 >> $@
		echo set result [get_property STATUS [get_runs impl_1]] >> $@
		echo set keyword [lindex [split '$$'result " "] end] >> $@
		echo if { '$$'keyword != \"Complete!\" } { >> $@
		echo     exit 1 >> $@
		echo } >> $@
		echo $(TCL_CLOSE) >> $@

par: synthesize par.tcl
		$(SYN_PRE_PAR_CMD)
		$(TCL_INTERPRETER) $@.tcl
		$(SYN_POST_PAR_CMD)
		touch $@

bitstream.tcl:
		echo $(TCL_OPEN) >> $@
		echo launch_runs impl_1 -to_step write_bitstream >> $@
		echo wait_on_run impl_1 >> $@
		echo $(TCL_CLOSE) >> $@

bitstream: par bitstream.tcl
		$(SYN_PRE_BITSTREAM_CMD)
		$(TCL_INTERPRETER) $@.tcl
		$(SYN_POST_BITSTREAM_CMD)
		touch $@

CLEAN_TARGETS := $(LIBS) .Xil *.jou *.log *.pb *.dmp $(PROJECT).cache $(PROJECT).data work $(PROJECT).runs $(PROJECT).hw $(PROJECT).ip_user_files $(PROJECT_FILE)

clean:
		rm -rf $(CLEAN_TARGETS)
		rm -rf project synthesize translate map par bitstream
		rm -rf project.tcl synthesize.tcl translate.tcl map.tcl par.tcl bitstream.tcl files.tcl

mrproper: clean
		rm -rf *.bit *.bin

.PHONY: mrproper clean all
