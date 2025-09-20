<customHeader>
    <Toolbar id="headerToolbar">
        <Image src="path/to/your/logo.png" alt="Company Logo" width="50px" /> <!-- Replace with your actual logo URL or path -->
        <Text text="Marius Pedersen" class="sapUiSmallMarginBegin" />
        <ToolbarSpacer />
        <Button press=".onSetEnglish" type="Transparent" tooltip="English"> <!-- Added tooltip for accessibility -->
            <layoutData><OverflowToolbarLayoutData priority="NeverOverflow" /></layoutData>
            <content>
                <Image src="https://upload.wikimedia.org/wikipedia/en/thumb/a/ae/Flag_of_the_United_Kingdom.svg/1280px-Flag_of_the_United_Kingdom.svg.png" 
                       alt="United Kingdom Flag (English)" 
                       width="24px" 
                       height="12px" /> <!-- Aspect ratio preserved; adjust size if needed -->
            </content>
        </Button>
        <Button press=".onSetDanish" type="Transparent" tooltip="Danish">
            <layoutData><OverflowToolbarLayoutData priority="NeverOverflow" /></layoutData>
            <content>
                <Image src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Flag_of_Denmark.svg/1280px-Flag_of_Denmark.svg.png" 
                       alt="Denmark Flag (Danish)" 
                       width="24px" 
                       height="18px" /> <!-- Aspect ratio preserved -->
            </content>
        </Button>
    </Toolbar>
</customHeader>
