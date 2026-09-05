from petropandas import pd

df = pd.read_excel("data/avg_pelite/microprobe_data.xlsx", sheet_name="Bulk", index_col=0)
df.bulk.TCbulk(H2O=4, oxygen=0.15)
df.bulk.MAGEMin(H2O=4, oxygen=0.15)
