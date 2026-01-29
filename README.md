In ABAP Development Tools öffnen
Datendefinition I_SALESORDERITEM [RI4, 442]
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81
82
83
84
85
86
87
88
89
90
91
92
93
94
95
96
97
98
99
100
101
102
103
104
105
106
107
108
109
110
111
112
113
114
115
116
117
118
119
120
121
122
123
124
125
126
127
128
129
130
131
132
133
134
135
136
137
138
139
140
141
142
143
144
145
146
147
148
149
150
151
152
153
154
155
156
157
158
159
160
161
162
163
164
165
166
167
168
169
170
171
172
173
174
175
176
177
178
179
180
181
182
183
184
185
186
187
188
189
190
191
192
193
194
195
196
197
198
199
200
201
202
203
204
205
206
207
208
209
210
211
212
213
214
215
216
217
218
219
220
221
222
223
224
225
226
227
228
229
230
231
232
233
234
235
236
237
238
239
240
241
242
243
244
245
246
247
248
249
250
251
252
253
254
255
256
257
258
259
260
261
262
263
264
265
266
267
268
269
270
271
272
273
274
275
276
277
278
279
280
281
282
283
284
285
286
287
288
289
290
291
292
293
294
295
296
297
298
299
300
301
302
303
304
305
306
307
308
309
310
311
312
313
314
315
316
317
318
319
320
321
322
323
324
325
326
327
328
329
330
331
332
333
334
335
336
337
338
339
340
341
342
343
344
345
346
347
348
349
350
351
352
353
354
355
356
357
358
359
360
361
362
363
364
365
366
367
368
369
370
371
372
373
374
375
376
377
378
379
380
381
382
383
384
385
386
387
388
389
390
391
392
393
394
395
396
397
398
399
400
401
402
403
404
405
406
407
408
409
410
411
412
413
414
415
416
417
418
419
420
421
422
423
424
425
426
427
428
429
430
431
432
433
434
435
436
437
438
439
440
441
442
443
444
445
446
447
448
449
450
451
452
453
454
455
456
457
458
459
460
461
462
463
464
465
466
467
468
469
470
471
472
473
474
475
476
477
478
479
480
481
482
483
484
485
486
487
488
489
490
491
492
493
494
495
496
497
498
499
500
501
502
503
504
505
506
507
508
509
510
511
512
513
514
515
516
517
518
519
520
521
522
523
524
525
526
527
528
529
530
531
532
533
534
535
536
537
538
539
540
541
542
543
544
545
546
547
548
549
550
551
552
553
554
555
556
557
558
559
560
561
562
563
564
565
566
567
568
569
570
571
572
573
574
575
576
577
578
579
580
581
582
583
584
585
586
587
588
589
590
591
592
593
594
595
596
597
598
599
600
601
602
603
604
605
606
607
608
609
610
611
612
613
614
615
616
617
618
619
620
621
622
623
624
625
626
627
628
629
630
631
632
633
634
635
636
637
638
639
640
641
642
643
644
645
646
647
648
649
650
651
652
653
654
655
656
657
658
659
660
661
662
663
@ClientHandling.algorithm: #SESSION_VARIABLE
@EndUserText.label: 'Sales Order Item'
@VDM: {
  viewType: #BASIC,
  lifecycle.contract.type: #PUBLIC_LOCAL_API
}
@AccessControl: {
  authorizationCheck: #CHECK,
  personalData.blocking: #('TRANSACTIONAL_DATA'),
  privilegedAssociations: [ '_CreatedByUser', '_BusinessAreaText','_MaterialText','_ProductText','_OrigMaterialText','_ShippingPointText' ]
}
@AbapCatalog: {
  sqlViewName:            'ISDSLSORDERITEM',
  preserveKey: true,
  compiler.compareFilter: true
}
@ObjectModel: {
  representativeKey: 'SalesOrderItem',
  usageType: {
    dataClass:      #TRANSACTIONAL,
    serviceQuality: #B,
    sizeCategory:   #XL
  },
  supportedCapabilities: [ #ANALYTICAL_DIMENSION, #CDS_MODELING_ASSOCIATION_TARGET, #SQL_DATA_SOURCE, #CDS_MODELING_DATA_SOURCE, #EXTRACTION_DATA_SOURCE ],
  modelingPattern: [ #ANALYTICAL_DIMENSION ]
}
@Metadata.ignorePropagatedAnnotations: true
@Analytics.dataCategory: #DIMENSION
@Metadata.allowExtensions: true
@ObjectModel.sapObjectNodeType.name: 'SalesOrderItem'
 
@Analytics: {
    dataExtraction: {
        enabled: true,
        delta.changeDataCapture: {
            mapping:[
                {
                    table: 'vbap', role: #MAIN,
                    viewElement: ['SalesOrder', 'SalesOrderItem'],
                    tableElement: ['vbeln', 'posnr']
                },
                {
                    table: 'vbak', role: #LEFT_OUTER_TO_ONE_JOIN,
                    viewElement: ['SalesOrder'],
                    tableElement: ['vbeln']},
                {
                    table: 'vbkd', role: #LEFT_OUTER_TO_ONE_JOIN,
                    viewElement: ['SalesOrder', 'SalesOrderItem'],
                    tableElement: ['vbeln', 'posnr']
                },
                {
                    table: 'veda', role: #LEFT_OUTER_TO_ONE_JOIN,
                    viewElement: ['SalesOrder', 'SalesOrderItem'],
                    tableElement: ['vbeln', 'vposn']
                }
            ]
        }
    }
 }
 
define view I_SalesOrderItem
  as select from I_SalesDocumentItem as SalesDocumentItem
 
 
  //--[ GENERATED:012:GFBfhxvv7jY4otz}RhcFP0
  association [0..*] to I_BusinessAreaText             as _BusinessAreaText  on  $projection.BusinessArea = _BusinessAreaText.BusinessArea
  association [0..*] to I_MaterialText                 as _MaterialText      on  $projection.Material = _MaterialText.Material
  association [0..*] to I_ProductText                  as _ProductText       on  $projection.Product = _ProductText.Product
  association [0..*] to I_MaterialText                 as _OrigMaterialText  on  $projection.OriginallyRequestedMaterial = _OrigMaterialText.Material
  association [0..*] to I_ShippingPointText            as _ShippingPointText on  $projection.ShippingPoint = _ShippingPointText.ShippingPoint
  // ]--GENERATED
  association [1..1] to I_SalesOrder                   as _SalesOrder        on  $projection.SalesOrder = _SalesOrder.SalesOrder
  association [0..*] to I_SalesOrderScheduleLine       as _ScheduleLine      on  $projection.SalesOrder     = _ScheduleLine.SalesOrder
                                                                             and $projection.SalesOrderItem = _ScheduleLine.SalesOrderItem
  association [0..*] to I_SalesOrderItemPartner        as _Partner           on  $projection.SalesOrder     = _Partner.SalesOrder
                                                                             and $projection.SalesOrderItem = _Partner.SalesOrderItem
  association [0..*] to I_SalesOrderItemPricingElement as _PricingElement    on  $projection.SalesOrder     = _PricingElement.SalesOrder
                                                                             and $projection.SalesOrderItem = _PricingElement.SalesOrderItem
  association [0..1] to I_SalesOrderItem               as _HigherLevelItem   on  $projection.SalesOrder      = _HigherLevelItem.SalesOrder
                                                                             and $projection.HigherLevelItem = _HigherLevelItem.SalesOrderItem
  association [0..1] to I_SalesOrderItemBillingPlan    as _ItemBillingPlan   on  $projection.SalesOrder     = _ItemBillingPlan.SalesOrder
                                                                             and $projection.SalesOrderItem = _ItemBillingPlan.SalesOrderItem
                                                                             and $projection.BillingPlan    = _ItemBillingPlan.BillingPlan
 
  association [0..*] to I_SalesOrderItmPrecdgProcFlow     as _PrecedingProcFlowDocItem  on  _PrecedingProcFlowDocItem.SalesOrder     = $projection.SalesOrder
                                                                                        and _PrecedingProcFlowDocItem.SalesOrderItem = $projection.SalesOrderItem
 
 
  association [0..*] to I_SalesOrderItmSubsqntProcFlow    as _SubsequentProcFlowDocItem on  _SubsequentProcFlowDocItem.SalesOrder     = $projection.SalesOrder
                                                                                        and _SubsequentProcFlowDocItem.SalesOrderItem = $projection.SalesOrderItem
 
 
  //Extensibility
  association [0..1] to E_SalesDocumentItemBasic       as _Extension         on  SalesDocumentItem.SalesDocument     = _Extension.SalesDocument
                                                                             and SalesDocumentItem.SalesDocumentItem = _Extension.SalesDocumentItem
 
{
      //Key
      @ObjectModel.foreignKey.association: '_SalesOrder'
  key cast(SalesDocument as vdm_sales_order preserving type)      as SalesOrder,
      @ObjectModel.text.element: 'SalesOrderItemText'
  key cast(SalesDocumentItem as sales_order_item preserving type) as SalesOrderItem,
      SalesDocumentItemUUID                                       as SalesOrderItemUUID,
 
      // Category
      @ObjectModel.foreignKey.association: '_ItemCategory'
      SalesDocumentItemCategory                                   as SalesOrderItemCategory,
      SalesDocumentItemType                                       as SalesOrderItemType,
      IsReturnsItem,
 
      //Admin
      CreatedByUser,
      @Semantics.systemDate.createdAt: true
      CreationDate,
      CreationTime,
      @Semantics.systemDate.lastChangedAt: true
      LastChangeDate,
 
      //Org
      @ObjectModel.foreignKey.association: '_Division'
      Division,
 
      //Product
      //--[ GENERATED:012:GFBfhxvv7jY4otz}RhcFP0
      @Consumption.valueHelpDefinition: [
        { entity:  { name:    'I_MaterialStdVH',
                     element: 'Material' }
        }]
      @ObjectModel.text.association: '_MaterialText'
      // ]--GENERATED
      @ObjectModel.foreignKey.association: '_Material'
      @VDM.lifecycle.status: #DEPRECATED
      @VDM.lifecycle.successor: 'Product'
      @API.element.releaseState: #DEPRECATED
      @API.element.successor: 'Product'
      Material,
      //--[ GENERATED:012:GFBfhxvv7jY4otz}RhcFP0
 
      @Consumption.valueHelpDefinition: [
        { entity:  { name:    'I_ProductStdVH',
                     element: 'Product' }
        }]
      @ObjectModel.text.association: '_ProductText'
      @Analytics.internalName: #LOCAL
      @ObjectModel.foreignKey.association: '_Product'
      Product,
 
      @Consumption.valueHelpDefinition: [
        { entity:  { name:    'I_ProductStdVH',
                     element: 'Product' }
        }]
      @ObjectModel.text.association: '_OrigMaterialText'
      // ]--GENERATED
      @ObjectModel.foreignKey.association: '_OriginallyRequestedMaterial'
      OriginallyRequestedMaterial,
      MaterialByCustomer,
      InternationalArticleNumber,
      //--[ GENERATED:012:GFBfhxvv7jY4otz}RhcFP0
      @Consumption.valueHelpDefinition: [
        { entity:  { name:    'I_BatchStdVH',
                     element: 'Batch' },
          additionalBinding: [{ localElement: 'Plant',
                                element: 'Plant' },
                              { localElement: 'Material',
                                element: 'Material' }]
        }]
      // ]--GENERATED
      @ObjectModel.foreignKey.association: '_Batch'
      Batch,
      @ObjectModel.foreignKey.association: '_ProductHierarchyNode'
      ProductHierarchyNode,
      ProductCatalog,
      @ObjectModel.foreignKey.association: '_MaterialSubstitutionReason'
      MaterialSubstitutionReason,
      @ObjectModel.foreignKey.association: '_MaterialGroup'
      @VDM.lifecycle.status: #DEPRECATED
      @VDM.lifecycle.successor: 'ProductGroup'
      MaterialGroup,
      @ObjectModel.foreignKey.association: '_ProductGroup'
      @Analytics.internalName: #LOCAL
      ProductGroup,
      @ObjectModel.foreignKey.association: '_MaterialPricingGroup'
      MaterialPricingGroup,
      @ObjectModel.foreignKey.association: '_AdditionalMaterialGroup1'
      AdditionalMaterialGroup1,
      @ObjectModel.foreignKey.association: '_AdditionalMaterialGroup2'
      AdditionalMaterialGroup2,
      @ObjectModel.foreignKey.association: '_AdditionalMaterialGroup3'
      AdditionalMaterialGroup3,
      @ObjectModel.foreignKey.association: '_AdditionalMaterialGroup4'
      AdditionalMaterialGroup4,
      @ObjectModel.foreignKey.association: '_AdditionalMaterialGroup5'
      AdditionalMaterialGroup5,
      @ObjectModel.foreignKey.association: '_Plant'
      Plant,
      @ObjectModel.foreignKey.association: '_OriginalPlant'
      OriginalPlant,
      @ObjectModel.foreignKey.association: '_StorageLocation'
      StorageLocation,
      DeliveryGroup,
      ProductConfiguration,
      BOMExplosionDate,
 
      //Sales
      @Semantics.text: true
      SalesDocumentItemText                                       as SalesOrderItemText,
      PurchaseOrderByCustomer,
      PurchaseOrderByShipToParty,
      @Analytics.internalName: #LOCAL
      CustomerPurchaseOrderDate,
      UnderlyingPurchaseOrderItem,
      ExternalItemID,
      @Semantics.quantity.unitOfMeasure: 'OrderQuantityUnit'
      OrderQuantity,
      @Semantics.unitOfMeasure: true
      @ObjectModel.foreignKey.association: '_OrderQuantityUnit'
      OrderQuantityUnit,
      OrderToBaseQuantityDnmntr,
      OrderToBaseQuantityNmrtr,
      @Semantics.quantity.unitOfMeasure: 'TargetQuantityUnit'
      TargetQuantity,
      @Semantics.unitOfMeasure: true
      @ObjectModel.foreignKey.association: '_TargetQuantityUnit'
      TargetQuantityUnit,
      TargetToBaseQuantityDnmntr,
      TargetToBaseQuantityNmrtr,
      @Semantics.quantity.unitOfMeasure: 'OrderQuantityUnit'
      ConfdDelivQtyInOrderQtyUnit,
      @Semantics.quantity.unitOfMeasure: 'OrderQuantityUnit'
      TargetDelivQtyInOrderQtyUnit,
      @Semantics.quantity.unitOfMeasure: 'BaseUnit'
      ConfdDeliveryQtyInBaseUnit,
      @Semantics.unitOfMeasure: true
      @ObjectModel.foreignKey.association: '_BaseUnit'
      BaseUnit,
      @Semantics.quantity.unitOfMeasure: 'OrderQuantityUnit'
      CommittedDelivQtyInOrdQtyUnit,
      CommittedDelivCreationDate,
      CommittedDeliveryDate,
      @Semantics.quantity.unitOfMeasure: 'RequestedQuantityUnit'
      RequestedQuantity,
      @Semantics.unitOfMeasure: true
      @ObjectModel.foreignKey.association: '_RequestedQuantityUnit'
      RequestedQuantityUnit,
      @Semantics.quantity.unitOfMeasure: 'BaseUnit'
      RequestedQuantityInBaseUnit,
 
      @Semantics.quantity.unitOfMeasure: 'ItemWeightUnit'
      ItemGrossWeight,
      @Semantics.quantity.unitOfMeasure: 'ItemWeightUnit'
      ItemNetWeight,
      @Semantics.unitOfMeasure: true
      @ObjectModel.foreignKey.association: '_ItemWeightUnit'
      ItemWeightUnit,
      @Semantics.quantity.unitOfMeasure: 'ItemVolumeUnit'
      ItemVolume,
      @Semantics.unitOfMeasure: true
      @ObjectModel.foreignKey.association: '_ItemVolumeUnit'
      ItemVolumeUnit,
 
      ServicesRenderedDate,
      @ObjectModel.foreignKey.association: '_SalesDistrict'
      SalesDistrict,
      //--[ GENERATED:012:GFBfhxvv7jY4otz}RhcFP0
      @Consumption.valueHelpDefinition: [
        { entity:  { name:    'I_SalesDealStdVH',
                     element: 'SalesDeal' }
        }]
      // ]--GENERATED
      @ObjectModel.foreignKey.association: '_SalesDeal'
      SalesDeal,
      SalesPromotion,
      RetailPromotion,
      @ObjectModel.foreignKey.association: '_CustomerGroup'
      CustomerGroup,
      @ObjectModel.foreignKey.association: '_SalesDocumentRjcnReason'
      SalesDocumentRjcnReason,
      RequirementSegment,
 
      SlsDocIsRlvtForProofOfDeliv,
 
      // Pricing
      @DefaultAggregation: #SUM
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      NetAmount,
      @Semantics.currencyCode: true
      @ObjectModel.foreignKey.association: '_TransactionCurrency'
      TransactionCurrency,
      PricingDate,
      PricingReferenceMaterial,
      ExchangeRateDate,
      PriceDetnExchangeRate,
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      NetPriceAmount,
      @Semantics.quantity.unitOfMeasure: 'NetPriceQuantityUnit'
      NetPriceQuantity,
      @Semantics.unitOfMeasure: true
      @ObjectModel.foreignKey.association: '_NetPriceQuantityUnit'
      NetPriceQuantityUnit,
 
      @ObjectModel.foreignKey.association: '_StatisticalValueControl'
      StatisticalValueControl,
 
      SalesDocumentItemProcgCode,
 
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      TaxAmount,
      ProductTaxClassification1,
      ProductTaxClassification2,
      ProductTaxClassification3,
      ProductTaxClassification4,
      ProductTaxClassification5,
      ProductTaxClassification6,
      ProductTaxClassification7,
      ProductTaxClassification8,
      ProductTaxClassification9,
      @Analytics.internalName: #LOCAL
      @ObjectModel.foreignKey.association: '_MatlAccountAssignmentGroup'
      MatlAccountAssignmentGroup,
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      CostAmount,
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      Subtotal1Amount,
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      Subtotal2Amount,
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      Subtotal3Amount,
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      Subtotal4Amount,
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      Subtotal5Amount,
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      Subtotal6Amount,
      SalesDocumentCondition                                      as SalesOrderCondition,
      @Analytics.internalName: #LOCAL
      @ObjectModel.foreignKey.association: '_CustomerPriceGroup'
      CustomerPriceGroup,
 
      @ObjectModel.foreignKey.association: '_ConditionGroup1'
      CustomerConditionGroup1,
      @ObjectModel.foreignKey.association: '_ConditionGroup2'
      CustomerConditionGroup2,
      @ObjectModel.foreignKey.association: '_ConditionGroup3'
      CustomerConditionGroup3,
      @ObjectModel.foreignKey.association: '_ConditionGroup4'
      CustomerConditionGroup4,
      @ObjectModel.foreignKey.association: '_ConditionGroup5'
      CustomerConditionGroup5,
 
      //Shipping
      //--[ GENERATED:012:GFBfhxvv7jY4otz}RhcFP0
      @Consumption.valueHelpDefinition: [
        { entity:  { name:    'I_ShippingPointStdVH',
                     element: 'ShippingPoint' }
        }]
      @ObjectModel.text.association: '_ShippingPointText'
      // ]--GENERATED
      @ObjectModel.foreignKey.association: '_ShippingPoint'
      ShippingPoint,
      @ObjectModel.foreignKey.association: '_ShippingType'
      ShippingType,
      InventorySpecialStockType,
      @ObjectModel.foreignKey.association: '_DeliveryPriority'
      DeliveryPriority,
      @ObjectModel.foreignKey.association: '_Route'
      Route,
      DeliveryDateQuantityIsFixed,
      DeliveryDateTypeRule,
      @Analytics.internalName: #LOCAL
      @ObjectModel.foreignKey.association: '_PartialDeliveryItem'
      PartialDeliveryIsAllowed,
      MaxNmbrOfPartialDelivery,
      UnlimitedOverdeliveryIsAllowed,
      OverdelivTolrtdLmtRatioInPct,
      UnderdelivTolrtdLmtRatioInPct,
      @Semantics.quantity.unitOfMeasure: 'BaseUnit'
      MinDeliveryQtyInBaseUnit,
      OpenDeliveryLeadingUnitCode,
      ItemIsDeliveryRelevant,
      ReceivingPoint,
 
      //Incoterms
      @ObjectModel.foreignKey.association: '_IncotermsVersion'
      IncotermsVersion,
      @ObjectModel.foreignKey.association: '_IncotermsClassification'
      IncotermsClassification,
      IncotermsTransferLocation,
      IncotermsLocation1,
      IncotermsLocation2,
      SalesDocumentItem.OrderCombinationIsAllowed,
 
      //Payment
      @ObjectModel.foreignKey.association: '_CustomerPaymentTerms'
      CustomerPaymentTerms,
      PaymentMethod,
      FixedValueDate,
      AdditionalValueDays,
 
      //Fashion
      ProductSeasonYear,
      ProductSeason,
      ProductCollection,
      ProductTheme,
      FashionCancelDate,
      ProductCharacteristic1,
      ProductCharacteristic2,
      ProductCharacteristic3,
      ShippingGroupNumber,
      ShippingGroupRule,
 
      //Billing
      BillingDocumentDate,
      @ObjectModel.foreignKey.association: '_ItemIsBillingRelevant'
      ItemIsBillingRelevant,
      @ObjectModel.foreignKey.association: '_ItemBillingBlockReason'
      ItemBillingBlockReason,
      BillingPlan,
 
      //Accounting
      FiscalYear,
      FiscalPeriod,
      @ObjectModel.foreignKey.association: '_CustomerAccountAssgmtGroup'
      CustomerAccountAssignmentGroup,
      //--[ GENERATED:012:GFBfhxvv7jY4otz}RhcFP0
      @Consumption.valueHelpDefinition: [
        { entity:  { name:    'I_BusinessAreaStdVH',
                     element: 'BusinessArea' }
        }]
      @ObjectModel.text.association: '_BusinessAreaText'
      // ]--GENERATED
      @ObjectModel.foreignKey.association: '_BusinessArea'
      BusinessArea,
      @Analytics.internalName: #LOCAL
      ControllingArea,
      @Analytics.internalName: #LOCAL
      ProfitCenter,
      @Analytics.internalName: #LOCAL
      @ObjectModel.foreignKey.association: '_WBSElementBasicData'
      @API.element.releaseState: #DEPRECATED
      @API.element.successor: 'WBSElementInternalID'
      WBSElement,
      @Analytics.internalName: #LOCAL
      @ObjectModel.foreignKey.association: '_WBSElementBasicData_2'
      WBSElementInternalID,
      OrderID,
      ControllingObject,
      @API.element.releaseState: #DECOMMISSIONED
      @API.element.successor: 'ProfitabilitySegment_2'
      ProfitabilitySegment,
      ProfitabilitySegment_2,
      OriginSDDocument,
      OriginSDDocumentItem,
      AccountingExchangeRate,
      ContractAccount,
 
      //Reference
      @ObjectModel.foreignKey.association: '_ReferenceSDDocument'
      ReferenceSDDocument,
      @ObjectModel.foreignKey.association: '_ReferenceSDDocumentItem'
      ReferenceSDDocumentItem,
      @ObjectModel.foreignKey.association: '_ReferenceSDDocumentCategory'
      ReferenceSDDocumentCategory,
      @ObjectModel.foreignKey.association: '_HigherLevelItem'
      @Analytics.internalName: #LOCAL
      HigherLevelItem,
      HigherLevelItemUsage,
      CorrespncExternalReference,
      POCorrespncExternalReference,
 
      //Status
      @ObjectModel.foreignKey.association: '_SDProcessStatus'
      SDProcessStatus,
      @ObjectModel.foreignKey.association: '_DeliveryConfirmationStatus'
      DeliveryConfirmationStatus,
      @ObjectModel.foreignKey.association: '_PurchaseConfirmationStatus'
      PurchaseConfirmationStatus,
      @ObjectModel.foreignKey.association: '_TotalDeliveryStatus'
      TotalDeliveryStatus,
      @ObjectModel.foreignKey.association: '_DeliveryStatus'
      DeliveryStatus,
      @ObjectModel.foreignKey.association: '_DeliveryBlockStatus'
      DeliveryBlockStatus,
      @ObjectModel.foreignKey.association: '_OrderRelatedBillingStatus'
      OrderRelatedBillingStatus,
      @ObjectModel.foreignKey.association: '_BillingBlockStatus'
      BillingBlockStatus,
      @ObjectModel.foreignKey.association: '_ItemGeneralIncompletionStatus'
      ItemGeneralIncompletionStatus,
      @ObjectModel.foreignKey.association: '_ItemBillingIncompletionStatus'
      ItemBillingIncompletionStatus,
      @ObjectModel.foreignKey.association: '_PricingIncompletionStatus'
      PricingIncompletionStatus,
      @ObjectModel.foreignKey.association: '_ItemDelivIncompletionSts'
      ItemDeliveryIncompletionStatus,
      @ObjectModel.foreignKey.association: '_SDDocReferenceStatus'
      SDDocReferenceStatus,
      @ObjectModel.foreignKey.association: '_SDDocumentRejectionStatus'
      SDDocumentRejectionStatus,
      @ObjectModel.foreignKey.association: '_TotalSDDocReferenceStatus'
      TotalSDDocReferenceStatus,
      @ObjectModel.foreignKey.association: '_ChmlCmplncStatus'
      ChmlCmplncStatus,
      @ObjectModel.foreignKey.association: '_DangerousGoodsStatus'
      DangerousGoodsStatus,
      @ObjectModel.foreignKey.association: '_SafetyDataSheetStatus'
      SafetyDataSheetStatus,
      @ObjectModel.foreignKey.association: '_ItemDownPaymentStatus'
      ContractItemDownPaymentStatus         as SlsOrderItemDownPaymentStatus,
      @ObjectModel.foreignKey.association: '_TrdCmplncEmbargoSts'
      TrdCmplncEmbargoSts,
      @ObjectModel.foreignKey.association: '_TrdCmplncSnctndListChkSts'
      TrdCmplncSnctndListChkSts,
      @ObjectModel.foreignKey.association: '_OvrlTrdCmplncLegalCtrlChkSts'
      OvrlTrdCmplncLegalCtrlChkSts,
      @ObjectModel.foreignKey.association: '_AltvBsdConfSubstitutionSts'
      AltvBsdConfSubstitutionStatus,
 
      OmniChannelSalesPromotion,
      OmniChannelSalesPromotionRule,
 
      @ObjectModel.foreignKey.association: '_ConsumptionPosting'
      ConsumptionPosting,
 
      //PBS
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      CappedNetAmount,
      CappedNetAmtAlertThldInPct,
      //this field is relevant only for PBS scenario
      @Semantics.amount.currencyCode: 'TransactionCurrency'
      SalesDocumentItem.TotalStandAloneSellingPrice,
 
      //Association
      @ObjectModel.association.type: [#TO_COMPOSITION_PARENT,
                                      #TO_COMPOSITION_ROOT]
      _SalesOrder,
      @ObjectModel.association.type: [#TO_COMPOSITION_CHILD]
      _ScheduleLine,
      @ObjectModel.association.type: [#TO_COMPOSITION_CHILD]
      _Partner,
      @ObjectModel.association.type: [#TO_COMPOSITION_CHILD]
      _PricingElement,
      @ObjectModel.association.type: [#TO_COMPOSITION_CHILD]
      _ItemBillingPlan,
      _ItemCategory,
      _CreatedByUser,
      _Division,
      _Material,
      _Product,
      _OriginallyRequestedMaterial,
      _MaterialPlant,
      _Batch,
      _ProductHierarchyNode,
      _MaterialSubstitutionReason,
      _MaterialGroup,
      _ProductGroup,
      _MaterialPricingGroup,
      _AdditionalMaterialGroup1,
      _AdditionalMaterialGroup2,
      _AdditionalMaterialGroup3,
      _AdditionalMaterialGroup4,
      _AdditionalMaterialGroup5,
      _MatlAccountAssignmentGroup,
      _Plant,
      _OriginalPlant,
      _StorageLocation,
      _OrderQuantityUnit,
      _BaseUnit,
      _TargetQuantityUnit,
      _RequestedQuantityUnit,
      _ItemWeightUnit,
      _ItemVolumeUnit,
      _SalesDistrict,
      _SalesDeal,
      _CustomerGroup,
      _CustomerPriceGroup,
      _SalesDocumentRjcnReason,
      _TransactionCurrency,
      _NetPriceQuantityUnit,
      _StatisticalValueControl,
      _ShippingPoint,
      _ShippingType,
      _DeliveryPriority,
      _DeliveryDateTypeRule,
      _PartialDeliveryItem,
      _Route,
      _IncotermsClassification,
      _IncotermsVersion,
      _ItemIsBillingRelevant,
      _ItemBillingBlockReason,
      _CustomerPaymentTerms,
 
      _ConditionGroup1,
      _ConditionGroup2,
      _ConditionGroup3,
      _ConditionGroup4,
      _ConditionGroup5,
 
      //PaymentMethod,
      _CustomerAccountAssgmtGroup,
      _BusinessArea,
      @Consumption.filter.businessDate.at: true
      _ProfitCenter,
 
      @VDM.lifecycle.status: #DEPRECATED
      @VDM.lifecycle.successor: '_WBSElementBasicData_2'
      @API.element.releaseState: #DEPRECATED
      @API.element.successor: '_WBSElementBasicData_2'
      _WBSElementBasicData,
      _WBSElementBasicData_2,
      _ReferenceSDDocument,
      _ReferenceSDDocumentItem,
      _ReferenceSDDocumentCategory,
      _HigherLevelItem,
      _SDProcessStatus,
      _DeliveryConfirmationStatus,
      _PurchaseConfirmationStatus,
      _TotalDeliveryStatus,
      _DeliveryStatus,
      _DeliveryBlockStatus,
      _ContractItemDownPaymentStatus                 as _ItemDownPaymentStatus,
      _OrderRelatedBillingStatus,
      _BillingBlockStatus,
      _ItemGeneralIncompletionStatus,
      _ItemBillingIncompletionStatus,
      _PricingIncompletionStatus,
      _ItemDelivIncompletionSts,
      _SDDocReferenceStatus,
      _SDDocumentRejectionStatus,
      _TotalSDDocReferenceStatus,
      _ChmlCmplncStatus,
      _DangerousGoodsStatus,
      _SafetyDataSheetStatus,
      _OvrlTrdCmplncLegalCtrlChkSts,
      _AltvBsdConfSubstitutionSts,
      _TrdCmplncSnctndListChkSts,
      _TrdCmplncEmbargoSts,
 
      @VDM.lifecycle.status: #DEPRECATED
      @VDM.lifecycle.successor: '_ItemBillingPlan'
      @API.element.releaseState: #DEPRECATED
      @API.element.successor: '_ItemBillingPlan'
      _BillingPlan,
      _PrecedingProcFlowDocItem,
      _SubsequentProcFlowDocItem,
 
 
      _ConsumptionPosting,
      //--[ GENERATED:012:GFBfhxvv7jY4otz}RhcFP0
      @Consumption.hidden: true
      _BusinessAreaText,
      @Consumption.hidden: true
      _MaterialText,
      @Consumption.hidden: true
      _ProductText,
      @Consumption.hidden: true
      _OrigMaterialText,
      @Consumption.hidden: true
      _ShippingPointText
      // ]--GENERATED
 
}
where
  SalesDocumentItem.SDDocumentCategory = 'C';
