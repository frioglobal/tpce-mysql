#
# Makefile to build EGenLoader, EGenValidate and EGen driver support code.
#

# PORTABILITY NOTES
# EGen makes use of "standardized" printf/scanf format specifiers
# and constant declaration macros which are typically defined in
# <inttypes.h> on Un*x-like platforms.  In order to get EGen to
# compile on some platforms, it may be neccessary to set one (or
# both) of -D__STDC_FORMAT_MACROS and -D__STDC_CONSTANT_MACROS.

# Platform specific defines
#CXX=
CCFLAGS=-D__STDC_CONSTANT_MACROS -D__STDC_FORMAT_MACROS  -std=c++11
#LDFLAGS=
#AR=
#ARFLAGS=

# Application specific defines
APPDEFINES=-DCOMPILE_FLAT_FILE_LOAD

# Directory for source files
SRC = ../src
# Directory for project files
PRJ = ../prj
# Directory for intermediate files
OBJ = ../obj
# Directory for include files
INC = ../inc
# Directory for library files
LIB = ../lib
# Directory for executable files
EXE = ../bin

# Directory for reference source files
REFSRC = ../../Reference/src
# Directory for reference include files
REFINC = ../../Reference/inc

# Directory for current library files
CURLIB = ../../../lib

VPATH=$(SRC):$(OBJ):$(REFSRC):$(REFOBJ)
.SUFFIXES: .cpp
.cpp.o:
	$(CXX) $(CCFLAGS) $(APPDEFINES) -DTPCE=RefTPCE -c $< -o $(OBJ)/$@

# Specify each library as a single target
# with source and include files separates into different variables.

EGenDriverLib_lib =		libegen.a

EGenDriverCELib_src =		CE.cpp \
				CETxnMixGenerator.cpp \
				CETxnInputGenerator.cpp

EGenDriverCELib_obj =		$(EGenDriverCELib_src:.cpp=.o)

EGenDriverDMLib_src =		DM.cpp

EGenDriverDMLib_obj =		$(EGenDriverDMLib_src:.cpp=.o)


EGenDriverMEELib_src =		MEE.cpp \
				MEEPriceBoard.cpp \
				MEESecurity.cpp \
				MEETickerTape.cpp \
				MEETradingFloor.cpp \
				WheelTime.cpp

EGenDriverMEELib_obj =		$(EGenDriverMEELib_src:.cpp=.o)


EGenLogger_src =		BaseLogger.cpp EGenLogFormatterTab.cpp

EGenLogger_obj =		$(EGenLogger_src:.cpp=.o)


EGenTables_src =		AddressTable.cpp \
				CustomerSelection.cpp \
				CustomerTable.cpp \
				InputFlatFilesStructure.cpp \
				Person.cpp \
				ReadRowFunctions.cpp \
				TradeGen.cpp

EGenTables_obj =		$(EGenTables_src:.cpp=.o)


EGenUtilities_src =		DateTime.cpp \
				error.cpp \
				Random.cpp \
				Money.cpp \
				EGenVersion.cpp


EGenUtilities_obj =		$(EGenUtilities_src:.cpp=.o)

EGenUtilities_inc =		EGenUtilities_stdafx.h \
				DateTime.h \
				FixedArray.h \
				FixedMap.h \
				Random.h \
				TableConsts.h \
				error.h


EGenTest_src =			EGenTest.cpp EGenTestSUT.cpp EGenTestSuiteOne.cpp EGenTestSuiteTwo.cpp

EGenTest_obj =			$(EGenTest_src:.cpp=.o)


# Using pattern rules that were defined earlier. 
# All options are specified through the variables.

all:				EGenDriverLib EGenTest

EGenTest:			EGenDriverLib \
				$(EGenTest_obj)
	cd $(OBJ); \
	$(CXX)  $(LDFLAGS) \
				$(EGenTest_obj) \
				$(LIB)/$(EGenDriverLib_lib) \
				$(CURLIB)/$(EGenDriverLib_lib) \
				$(LIBS) \
				-o $(EXE)/$@; \
	cd $(PRJ); \
	ls -al $(EXE)

EGenDriverLib:			EGenDriverCELib \
				EGenDriverDMLib \
				EGenDriverMEELib \
				EGenUtilities \
				EGenLogger \
				EGenTables
	cd $(OBJ); \
	$(AR) $(ARFLAGS) $(LIB)/$(EGenDriverLib_lib) \
				$(EGenDriverCELib_obj) \
				$(EGenDriverDMLib_obj) \
				$(EGenDriverMEELib_obj) \
				$(EGenUtilities_obj) \
				$(EGenLogger_obj) \
				$(EGenTables_obj); \
	cd $(PRJ); \
	ls -l  $(LIB)

EGenDriverCELib:		$(EGenDriverCELib_obj)

EGenDriverDMLib:		$(EGenDriverDMLib_obj)

EGenDriverMEELib:		$(EGenDriverMEELib_obj)

EGenLogger:			$(EGenLogger_obj)

EGenUtilities:			$(EGenUtilities_obj)

EGenTables:			$(EGenTables_obj) 

clean:
	cd $(OBJ); \
	rm -f			$(EGenUtilities_obj) \
				$(EGenDriverCELib_obj) \
				$(EGenDriverDMLib_obj) \
				$(EGenDriverMEELib_obj) \
				$(EGenLogger_obj) \
				$(EGenTables_obj) \
				$(EGenTest_obj); \
	cd $(LIB); \
	rm -f			$(EGenDriverLib_lib); \
	cd $(EXE); \
	rm -f			EGenTest; \
	cd $(PRJ)
