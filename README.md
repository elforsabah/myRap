<VBox id="ltContainer"
      items="{
        path: '/ZI_WR_LOADTYPEVH',
        parameters: { $select: 'LoadType,LoadTypeText', $orderby: 'LoadType' }
      }">
  <Button
    class="loadTypeBtn"
    width="100%"
    text="{= ${LoadType} + ' - ' + ${LoadTypeText} }"
    press=".onChooseLoadType" />
</VBox>
