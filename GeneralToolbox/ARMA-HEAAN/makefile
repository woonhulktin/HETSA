# This sample should be run after make libHEAAN.a file in lib folder

# All Target
all: clean ARMA

# Tool invocations
ARMA:
	@echo 'Building target: $@'
	@echo 'Invoking: GCC C++ Linker'
	g++ arma.cpp -o ARMA -L/usr/local/lib ../../HEAAN/lib/libHEAAN.a -lntl -lgmp -lm -std=c++11 -O2 -I/../../HEAAN/src -pthread
	@echo 'Finished building target: $@'
	@echo ' '

# Other Targets
clean:
	rm -rf ARMA
