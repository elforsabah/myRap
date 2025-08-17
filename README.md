<Annotations Target="your.namespace.ZI_WR_WEIGHINGSESSION/LoadType">  <!-- Replace with your actual entity/property path -->
                <Annotation Term="Common.ValueList">
                    <Record>
                        <PropertyValue Property="Label" String="Load Type"/>
                        <PropertyValue Property="CollectionPath" String="LoadTypeValueHelpEntity"/>  <!-- Replace with your actual value help entity name from CDS, e.g., I_LoadTypeVH -->
                        <PropertyValue Property="ValueListWithFixedValues" Bool="true"/>  <!-- This forces dropdown rendering -->
                        <PropertyValue Property="Parameters">
                            <Collection>
                                <Record Type="Common.ValueListParameterOut">
                                    <PropertyValue Property="LocalDataProperty" PropertyPath="LoadType"/>
                                    <PropertyValue Property="ValueListProperty" String="LoadTypeKey"/>  <!-- Key field in VH -->
                                </Record>
                                <Record Type="Common.ValueListParameterDisplayOnly">
                                    <PropertyValue Property="ValueListProperty" String="LoadTypeDescription"/>  <!-- Text field in VH -->
                                </Record>
                            </Collection>
                        </PropertyValue>
                    </Record>
                </Annotation>
            </Annotations>
