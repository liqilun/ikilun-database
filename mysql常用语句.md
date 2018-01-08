CREATE TABLE mall.local_express_data(  
  recordid INT(11) NOT NULL AUTO_INCREMENT,
  express_info_id INT(11) NOT NULL,
  time VARCHAR(50),
  context VARCHAR(255),
  PRIMARY KEY (recordid),
  INDEX idx_express_info_id (express_info_id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert into sku_stock (version, product_id, sku_id, init_stock, stock, sold, batch_no, 
cost_price, add_time)
select sku_version,product_id,recordid,init_stock,stock,sold,CONCAT(DATE_FORMAT(SYSDATE(),'%y%m%d%H%i%S'),LPAD(recordid,4,'0')),
cost_price,SYSDATE()  from sku;

ALTER TABLE mall.product DROP COLUMN stock;

ALTER TABLE mall.gewa_commend ADD COLUMN smalllogo VARCHAR(200) NULL AFTER parent_id;


SELECT DATE_FORMAT(add_time,'%Y-%m-%d'),COUNT(1) FROM mall_order WHERE sh_status='paid_success' 
AND add_time>'2015-10-20'  GROUP BY DATE_FORMAT(add_time,'%Y-%m-%d') ORDER BY DATE_FORMAT(add_time,'%Y-%m-%d') DESC;


ALTER TABLE mall.mall_order   
  ADD COLUMN discount INT DEFAULT 0  NOT NULL AFTER express_status,
  ADD COLUMN discount_reason VARCHAR(500) NULL AFTER discount;
  
  
ALTER TABLE classify MODIFY COLUMN name varchar(100);


String sql = "SELECT COUNT(1) AS c,IFNULL(SUM(m.qty),0) AS qty,IFNULL(SUM(m.qty*m.unit_price),0) AS totalfee,IFNULL(SUM(m.total_cost),0) AS totalcost,"
			+ "IFNULL(SUM(CASE WHEN o.discount>0 THEN 1 ELSE 0 END),0)  AS dc,"
			+ "IFNULL(SUM(CASE WHEN o.discount>0 THEN m.qty ELSE 0 END),0)  AS dqty,"
			+ "IFNULL(SUM(CASE WHEN m.used_point>0 THEN 1 ELSE 0 END),0)  AS pc,"
			+ "IFNULL(SUM(CASE WHEN m.used_point>0 THEN m.qty ELSE 0 END),0)  AS pqty "
			+ "FROM mall_order_item m LEFT JOIN mall_order o ON m.mall_order_id=o.recordid WHERE o.add_time>=? AND o.add_time<=? and o.sh_status=? ";