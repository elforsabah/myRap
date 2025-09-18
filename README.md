extension implementation in class zbp_zpd_bp_r_pdservice unique;

extend behavior for Service
{
  determination ZZServiceCreateDetermination on modify { create; }

  determination SetPointOfOrigin on modify { create; }

}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
 field ( suppress ) zzpobjnr;

 field ( suppress ) point_of_origin;

}
