# Default directories
MAN_DIR = man
PDF_DIR = pdf
POD_DIR = pod
PO_DIR  = po

# fragmaster script location
FRAGMASTER = ../../fragmaster.pl

# Version
VERSION = $(shell grep "^\# Version:" $(FRAGMASTER) | sed 's/[^.[:digit:]]//g')

# Available translations
LANGS   = $(patsubst $(PO_DIR)/%.po,%,$(wildcard $(PO_DIR)/*.po))

# Translated man pages and pdffiles
MAN_1   = $(addprefix $(MAN_DIR)/fragmaster.,$(addsuffix .1,$(LANGS)))
PDF     = $(addprefix $(PDF_DIR)/fragmaster.,$(addsuffix .pdf,$(LANGS)))

default:
	@echo "No default action. Info about current status:"
	@echo "  Version: $(VERSION), languages: $(LANGS)"
	@echo "Useful targets:"
	@echo "  man    Create man pages: $(MAN_1)"
	@echo "  pdf    Create PDF files: $(PDF)"

man: $(MAN_DIR)/fragmaster.1   $(MAN_1)
pdf: $(PDF_DIR)/fragmaster.pdf $(PDF)

$(POD_DIR)/1/%/fragmaster.pod: $(FRAGMASTER) po4a.cfg
	po4a po4a.cfg

$(MAN_DIR)/fragmaster.1: $(FRAGMASTER)
	mkdir -p $(dir $@)
	pod2man --utf8 "$<" "$@"

$(MAN_DIR)/fragmaster.%.1: $(POD_DIR)/1/%/fragmaster.pod
	mkdir -p $(dir $@)
	pod2man --utf8 "$<" "$@"

$(PDF_DIR)/fragmaster.pdf: $(MAN_DIR)/fragmaster.1
	mkdir -p $(dir $@)
	man -t "$<" | ps2pdf - "$@"

$(PDF_DIR)/fragmaster.%.pdf: $(MAN_DIR)/fragmaster.%.1
	mkdir -p $(dir $@)
	man -t "$<" | ps2pdf - "$@"

clean:
	rm -rf $(POD_DIR) $(MAN_DIR) $(PDF_DIR)
