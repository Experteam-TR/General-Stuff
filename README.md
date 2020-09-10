# General-Stuff

Genşler buraya kategorize edemediğimiz şeyleri koyalım. İleride içerik dolmaya başlayınca buraları da detaylandırırız.

Select   To_date (cbi.bill_dt, 'dd.mm.yyyy') "Bill Date", cbi.bill_id "Bill Id",
         Decode (Trim (cbi.cr_note_fr_bill_id), Null, Null, 'Credit Note') "Tip",
         (Select Sum (Nvl (calc_amt, 0))
            From ci_bseg_calc_ln cbsl
           Where cbsl.bseg_id = cbs.bseg_id And Trim (cbsl.dst_id) Is Not Null) "Bseg Calc Amt",
         cbs.start_dt "Bseg Start", cbs.end_dt "Bseg End", To_date (c1ur.start_dttm, 'dd.mm.yyyy') "Ur Start",
         To_date (c1ur.end_dttm, 'dd.mm.yyyy') "Ur End", cbs.prem_id "Premise Id", cbs.bseg_id "Bill Seg Id",
         cbi.bill_cyc_cd "Bill Cycle", cbi.win_start_dt "Wnd Start Date", cbs.bseg_stat_flg "Bill Seg Stat",
         cbi.cre_dttm "Bill Creation", cbi.*
    From ci_bseg cbs, ci_bill cbi, c1_usage c1ur
   Where cbi.bill_id = cbs.bill_id(+)
-- And cbi.acct_id =
     And cbi.acct_id In (4692137931, 0318068568, 1103303889, 9276885521)
     And cbs.bseg_id = c1ur.bseg_id(+)
--     And Not Exists (Select 'nope'
--                       From ci_bill cbi2
--                      Where Nvl (cbi.cr_note_fr_bill_id, 'X') = cbi2.bill_id)
     And Nvl (Trim (cbs.bseg_stat_flg), 'Boş') In ('50', '30')
Order By cbi.bill_dt Desc, cbi.bill_id

