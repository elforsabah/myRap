<VBox id="step2LtContainer"
      items="{
        path: '/ZI_WR_SALESITEM_CONTRACTVH',
        parameters: {
          $$parameters: { 'P_SalesOrder': '{Vbeln}' },
          $select: 'SalesOrder,SalesOrderitem,Material,MaterialText,Language',
          $orderby: 'SalesOrder,SalesOrderitem'
        }
      }">
  <Button id="step2LtButton"
          class="loadTypeBtn"
          width="100%"
          text="{= ${Material} + ' - ' + ${MaterialText} }"
          press=".onChooseLoadType"/>
</VBox>
