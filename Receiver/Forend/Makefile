include ../../Makefile.in
ANIMATE_BIN=../../bin/animate.sh

# Forend Classes
CLASSES := TopBreak Revolver

# Forend Class Parts
TopBreak := ReceiverFront \
            Forend \
            BarrelCollar \
            Extractor
Revolver := ReceiverFront \
						FrameSpacer \
						BarrelSupport \
						Foregrip \
						CylinderCore \
						CylinderShell \
						ForendSpindleToggleLinkage \
						ForendSpindleToggleHandle

# Forend Presets
TopBreak_PRESETS := CAFE12 CAFE12+ FP37 
Revolver_PRESETS := ZZR6x12

PRESETS := $(addprefix TopBreak_,$(TopBreak_PRESETS)) $(addprefix Revolver_,$(Revolver_PRESETS))

# All Part STLs for all forend presets
TopBreak_STLS := $(addprefix TopBreak_, $(foreach PRESET,$(TopBreak_PRESETS),$(addsuffix .stl, $(addprefix $(addsuffix /, $(PRESET)),$(TopBreak)))))
Revolver_STLS := $(addprefix Revolver_, $(foreach PRESET,$(Revolver_PRESETS),$(addsuffix .stl, $(addprefix $(addsuffix /, $(PRESET)),$(Revolver)))))

PRESET_STLS := $(TopBreak_STLS) $(Revolver_STLS)
PRESET_HTML := $(addsuffix /README.html,$(PRESETS))
PRESET_STL_PREVIEWS := $(addsuffix .mp4, $(PRESET_STLS))

CLASS_STLS := $(addsuffix .stl,$(CLASSES))
CLASS_HTML := $(addsuffix /README.html,$(CLASSES))


.SECONDEXPANSION:
	
# Class/Preset dirs depend on their respective source
$(CLASSES) $(PRESETS): $$(addsuffix .scad,$$(firstword $$(subst _, ,$$@)))
	mkdir -p $@
	touch $@

# Preset STL depend on their respective source file
$(PRESET_STLS):
	$(eval CLASS_PRESET=$(firstword $(subst /, ,$@)))
	$(eval CLASS=$(shell echo $(CLASS_PRESET) | awk -F _ '{ print $$1 }'))
	$(eval PRESET=$(shell echo $(CLASS_PRESET) | awk -F _ '{ print $$2 }'))
	$(eval PART=$(basename $(notdir $@)))
	@echo Target: $@
	@echo Deps: $^
	@echo Class_Preset: $(CLASS_PRESET)
	@echo Class: $(CLASS)
	@echo Preset: $(PRESET)
	@echo Part: $(PART)
	$(OSBIN) $(OSOPTS) --render -D _RENDER=\"$(CLASS)_$(PART)\" \
		--projection=p --camera=0,400,400,0,100,100 \
		-o $@ -o $(CLASS_PRESET)/$(PART).png \
		-P $(PRESET) \
		$(CLASS).scad
	$(THUMBNAILBIN) $(CLASS_PRESET)/$(PART).png $(CLASS_PRESET)/$(PART)_thumb.jpg

# HTML file for the preset
$(PRESET_HTML): %/README.html : % %.md
	$(MDBIN) $(basename $^).md | sed 's/\.md/\.html/' > $@
$(CLASS_HTML): %/README.html : % %.md
	$(MDBIN) $(basename $^).md | sed 's/\.md/\.html/' > $@

all: README.html $(CLASSES) $(CLASS_HTML) $(PRESETS) $(PRESET_HTML) $(PRESET_STLS) $(PRESET_STL_PREVIEWS)