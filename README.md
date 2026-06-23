extend view entity /PLCE/R_PDTourExtCustom
  with /PLCE/E_PDTourBmsStatus
{
  /plce/tpdtourcst.zz_bms_status    as ZzBmsStatus,

  case /plce/tpdtourcst.zz_bms_status
    when 'SENT'        then 3    " green
    when 'FREIGEGEBEN' then 3    " green
    when 'PARTIAL'     then 2    " orange/yellow
    when 'BEGONNEN'    then 2    " orange
    when 'BEENDET'     then 3    " green
    when 'ERROR'       then 1    " red
    when 'STORNIERT'   then 0    " grey/neutral
    else                    0
  end                        as ZzBmsCriticality
}
