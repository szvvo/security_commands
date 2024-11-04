
- using shodan to get latest cve releases
```bash 
# using jq
curl -s https://cvedb.shodan.io/cves | jq '.cves[] | {cve_id,summary,published_time}' | jq -s 'sort_by(.published_time)'
```