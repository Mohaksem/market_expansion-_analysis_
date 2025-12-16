USE marketing_data;

SELECT
    c.CampaignName,
    SUM(m.Clicks) AS TotalClicks,
    SUM(m.Impressions) AS TotalImpressions,
    (CAST(SUM(m.Clicks) AS DECIMAL(10, 2)) * 100.0) / SUM(m.Impressions) AS CTR_Percentage,
    SUM(m.Cost) / SUM(m.Clicks) AS CPC
FROM
    campaigns c                
JOIN
    performance_metrics m      
ON c.CampaignID = m.CampaignID
WHERE
    c.StartDate >= DATE_SUB(DATE_FORMAT(NOW(), '%Y-%m-01'), INTERVAL 1 MONTH)
    AND c.StartDate < DATE_FORMAT(NOW(), '%Y-%m-01')
GROUP BY
    c.CampaignName
HAVING
    SUM(m.Impressions) > 0
ORDER BY
    CTR_Percentage DESC
LIMIT 0, 1000;
