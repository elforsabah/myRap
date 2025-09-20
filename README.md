<customHeader>
    <Toolbar id="headerToolbar">
        <Image src="path/to/your/logo.png" alt="Company Logo" width="50px" /> <!-- Replace with your actual logo URL or path -->
        <Text text="Marius Pedersen" class="sapUiSmallMarginBegin" />
        <ToolbarSpacer />
        <Button icon="https://upload.wikimedia.org/wikipedia/en/thumb/a/ae/Flag_of_the_United_Kingdom.svg/1280px-Flag_of_the_United_Kingdom.svg.png" 
                press=".onSetEnglish" 
                type="Transparent" 
                tooltip="English" /> <!-- Icon as flag; add text="EN" if desired -->
        <Button icon="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Flag_of_Denmark.svg/1280px-Flag_of_Denmark.svg.png" 
                press=".onSetDanish" 
                type="Transparent" 
                tooltip="Danish" /> <!-- Icon as flag; add text="DK" if desired -->
    </Toolbar>
</customHeader>
