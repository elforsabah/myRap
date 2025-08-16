<Annotations Target="SAP__self.ZI_WR_WEIGHINGSESSION" xmlns="http://docs.oasis-open.org/odata/ns/edm">
    <Annotation Term="UI.FieldGroup" Qualifier="Identification">
        <Record Type="UI.FieldGroupType">
            <PropertyValue Property="Data">
                <Collection>
                    <Record Type="UI.DataField">
                        <PropertyValue Property="Value" Path="vbeln" />
                        <PropertyValue Property="Label" String="Delivery Number" />
                    </Record>
                    <Record Type="UI.DataField">
                        <PropertyValue Property="Value" Path="sessionid" />
                        <PropertyValue Property="Label" String="Session ID" />
                    </Record>
                    <!-- Add more for vehicle/driver ID fields if present in CDS -->
                </Collection>
            </PropertyValue>
        </Record>
    </Annotation>
    <Annotation Term="UI.FieldGroup" Qualifier="Weights">
        <Record Type="UI.FieldGroupType">
            <PropertyValue Property="Data">
                <Collection>
                    <Record Type="UI.DataField">
                        <PropertyValue Property="Value" Path="grossweight" />
                        <PropertyValue Property="Label" String="Gross Weight" />
                    </Record>
                    <Record Type="UI.DataField">
                        <PropertyValue Property="Value" Path="tareweight" />
                        <PropertyValue Property="Label" String="Tare Weight" />
                    </Record>
                    <Record Type="UI.DataField">
                        <PropertyValue Property="Value" Path="netweight" />
                        <PropertyValue Property="Label" String="Net Weight" />
                    </Record>
                </Collection>
            </PropertyValue>
        </Record>
    </Annotation>
    <!-- Add more qualifiers for other groups, e.g., Load Type or Steps -->
    <!-- Example for value help if needed: <Annotation Term="Common.ValueList" Qualifier="LoadType"> ... </Annotation> -->
</Annotations>
