app
NCC ncc = new NCC();

SessionMNG SMNG_1 = new SMNG();
SMNG.NewSession = listener.accepted();

SessionMNG SMNG_2 = new SMNG();
SMNG.NewSession = connector.accepted();

ncc.listener.start(10000);
ncc.connector.start(192.168.10.2, 10002);
ncc.connector.start(192.168.10.3, 10003);

//

Sessionmng sm = new sessionmng

sm.newsessionS

sm.addsessions